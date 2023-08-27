<script setup>
import TitleBar from "./components/header/TitleBar.vue";
import { NGrid, NGridItem, NIcon, NButton, NMessageProvider, NSpace } from 'naive-ui'
import { BulbOutline, BulbSharp } from '@vicons/ionicons5'
import Input from "./components/main/Input.vue"
import KeyBoard from "./components/main/Keyboard.vue"
import EndScreen from "./components/main/Endscreen.vue"
import Notification from "./components/main/Notification.vue";
import { ref, onMounted } from 'vue'

const inputs = [ref(""), ref(""), ref("")]
const inputsValidated = [ref(false), ref(false), ref(false)]
const notificationMgr = ref(null)
var gameFinished = ref(false)
var userID = ref("")
var gameID = ref("")

var currentInput = 0
var finalScore = 0

function loadGame() {
  const str = getCookie("gameDone")
  gameFinished.value = (str === "true");
  userID.value = getCookie("userid")
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
  gameID.value = id
  gameFinished.value = true
  setCookieAndExpireAtMidnight("gameDone", true)

  if (userID.value != "") {
    registerGameAPI()
  }
}

loadGame()
</script>

<template>
  <n-grid :cols="1" :y-gap="50">
    <n-grid-item>
      <TitleBar></TitleBar>
    </n-grid-item>
    <n-grid-item>
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
        <KeyBoard @type="type" @valid="registerGame" />
      </Suspense>
    </n-grid-item>
    <EndScreen v-model:show="gameFinished" @newUser="loadUser" :userID="userID"></EndScreen>
  </n-grid>
  <footer>
    <n-button circle>
      <template #icon>
        <n-icon>
          <BulbSharp />
        </n-icon>
      </template>
    </n-button>
  </footer>
  <n-message-provider placement="top-right">
    <Notification ref="notificationMgr"></Notification>
  </n-message-provider>
</template>

<style scoped></style>