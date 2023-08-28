<script setup>
import TitleBar from "./components/header/TitleBar.vue";
import { NGrid, NGridItem, NIcon, NButton, NMessageProvider, NDivider } from 'naive-ui'
import confetti from 'canvas-confetti'
import Input from "./components/main/Input.vue"
import KeyBoard from "./components/main/Keyboard.vue"
import EndScreen from "./components/main/Endscreen.vue"
import Notification from "./components/main/Notification.vue";
import { ref, onMounted } from 'vue'

const inputs = [ref(""), ref(""), ref("")]
const inputsValidated = [ref(false), ref(false), ref(false)]
const notificationMgr = ref(null)
const gameFinished = ref(false)
const showEndScreen = ref(false)
const userID = ref("")
const gameID = ref("")

var currentInput = 0
var finalScore = 0

function loadGame() {
  console.log(gameFinished.value)
  var str = getCookie("gameDone")
  gameFinished.value = (str === "true");
  userID.value = getCookie("userid")

  str = getCookie("words")
  if (str) {
    let jsInput = str.split(",")
    for (let index = 0; index < jsInput.length; index++) {
      const element = jsInput[index];
      inputs[index].value = element
    }
  }

  str = getCookie("inputValid")
  if (str) {
    let jsInput = str.split(",")
    for (let index = 0; index < jsInput.length; index++) {
      const element = jsInput[index];
      inputsValidated[index].value = element === "true"
    }
  }
  showEndScreen.value = gameFinished.value
}

function saveGame() {
  setCookieAndExpireAtMidnight("gameDone", true)

  let jsInput = []
  for (const key in inputs) {
    if (Object.hasOwnProperty.call(inputs, key)) {
      const element = inputs[key];
      jsInput.push(element.value)
    }
  }
  setCookieAndExpireAtMidnight("words", jsInput)

  let jsInputValidated = []
  for (const key in inputsValidated) {
    if (Object.hasOwnProperty.call(inputsValidated, key)) {
      const element = inputsValidated[key];
      jsInputValidated.push(element.value)
    }
  }
  setCookieAndExpireAtMidnight("inputValid", jsInputValidated)

}

// Cookie helper
function setCookieAndExpire(cname, cvalue, expire) {
  document.cookie = cname + "=" + cvalue + ";expires=" + expire.toUTCString();
}
function setCookieAndExpireAtMidnight(cname, cvalue) {
  let date = new Date();
  let midnight = new Date(date.getFullYear(), date.getMonth(), date.getDate(), 23, 59, 59);
  setCookieAndExpire(cname, cvalue, midnight)
}
function getCookie(cname) {
  let name = cname + "=";
  let ca = document.cookie.split(';');
  for (let i = 0; i < ca.length; i++) {
    let c = ca[i];
    while (c.charAt(0) == ' ') {
      c = c.substring(1);
    }
    if (c.indexOf(name) == 0) {
      return c.substring(name.length, c.length);
    }
  }
  return "";
}

// Event
function type(letter) {
  inputs[currentInput].value += letter
}
function clear(idx) {
  inputs[idx].value = ""
}
function wordDontExist() {
  clear(currentInput)
  notificationMgr.value.createMessage("Ce mot n'existe pas !", "error")
}
function newWord(score) {
  for (let index = 0; index < currentInput; index++) {
    const element = inputs[index];
    if (element.value === inputs[currentInput].value) {
      clear(currentInput)
      notificationMgr.value.createMessage("Mot déjà saisi !", "warning")
      return
    }
  }

  inputsValidated[currentInput].value = true
  finalScore += score
  if (currentInput < 2) {
    currentInput++
  }
}
async function loadUser() {
  userID.value = getCookie("userid")
  registerGameAPI()
  confetti({
    particleCount: 100,
    spread: 100,
    origin: { x: 0.5, y: 0.8 },
    zIndex: 999,
  });
}
async function registerGameAPI() {
  let response = await fetch("https://wordcraft-397020.ew.r.appspot.com/user/score", {
    method: "POST",
    body: JSON.stringify({
      user: userID.value,
      pick: gameID.value, score: finalScore
    })
  })
  if (!response.ok) {
    return Promise.reject(response);
  }
}
async function registerGame(id) {
  if (finalScore === 0) {
    notificationMgr.value.createMessage("Trouvez un mot d'abord !", "error")
    return;
  }

  gameID.value = id
  gameFinished.value = true
  showEndScreen.value = true
  saveGame()

  if (userID.value != "") {
    registerGameAPI()
    confetti({
      particleCount: 100,
      spread: 100,
      origin: { x: 0.5, y: 0.8 },
      zIndex: 999,
    })
  }
}

loadGame()
</script>

<template>
  <n-grid :cols="1" :y-gap="70">
    <n-grid-item>
      <TitleBar></TitleBar>
      <n-divider />
      <n-grid :cols="1" :y-gap="10">
        <n-grid-item :suffix="true">
          <Input :word="inputs[0].value" :isValidated="inputsValidated[0].value" @clear="clear(0)" @newWord="newWord"
            @notExist="wordDontExist" />
        </n-grid-item>
        <n-grid-item>
          <Input :word="inputs[1].value" :isValidated="inputsValidated[1].value" @clear="clear(1)" @newWord="newWord"
            @notExist="wordDontExist" />
        </n-grid-item>
        <n-grid-item>
          <Input :word="inputs[2].value" :isValidated="inputsValidated[2].value" @clear="clear(2)" @newWord="newWord"
            @notExist="wordDontExist" />
        </n-grid-item>
      </n-grid>
    </n-grid-item>
    <n-grid-item>
      <Suspense>
        <KeyBoard @type="type" @valid="registerGame" :gameFinished="gameFinished" :userID="userID" />
      </Suspense>
    </n-grid-item>
  </n-grid>

  <EndScreen v-model:show="showEndScreen" @newUser="loadUser" :userID="userID"></EndScreen>

  <n-message-provider placement="bottom-right">
    <Notification ref="notificationMgr"></Notification>
  </n-message-provider>
</template>

<style scoped></style>