<script setup>
import TitleBar from "./components/header/TitleBar.vue";
import { NGrid, NGridItem, NIcon, NButton } from 'naive-ui'
import { BulbOutline, BulbSharp } from '@vicons/ionicons5'
import Input from "./components/main/Input.vue"
import KeyBoard from "./components/main/Keyboard.vue"
import { ref } from 'vue'

const inputs = [ref(""), ref(""), ref("")]
var currentInput = 0
var finalScore = 0

function type(letter) {
  inputs[currentInput].value += letter
}
function clear(idx) {
  console.log("cls");
  inputs[idx].value = ""
}
function newWord(score) {
  finalScore += score
  if (currentInput < 2) {
    currentInput++
  }
}
async function registerGame(id) {
  let response = await fetch("https://wordcraft-397020.ew.r.appspot.com/user/score", {
    method: "POST",
    body: JSON.stringify({
      user: "63f3e957-0baf-499f-b449-88885325d7a4",
      pick: id, score: finalScore
    })
  })

  if (!response.ok) {
    return Promise.reject(response);
  }
}
</script>

<template>
  <header>
    <TitleBar></TitleBar>
  </header>
  <main>

    <n-grid :cols="1" :y-gap="10">
      <n-grid-item :suffix="true">
        <Input :word="inputs[0].value" @clear="clear(0)" @newWord="newWord" />
      </n-grid-item>
      <n-grid-item>
        <Input :word="inputs[1].value" @clear="clear(1)" @newWord="newWord" />
      </n-grid-item>
      <n-grid-item>
        <Input :word="inputs[2].value" @clear="clear(2)" @newWord="newWord" />
      </n-grid-item>
      <n-grid-item>
        <Suspense>
          <KeyBoard @type="type" @valid="registerGame" />
        </Suspense>
      </n-grid-item>
    </n-grid>
  </main>
  <footer>
    <n-button circle>
      <template #icon>
        <n-icon>
          <BulbSharp />
        </n-icon>
      </template>
    </n-button>
  </footer>
</template>

<style scoped></style>

