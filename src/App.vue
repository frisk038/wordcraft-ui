<script setup>
import TitleBar from "./components/header/TitleBar.vue";
import { NGrid, NGridItem } from 'naive-ui'
import Input from "./components/main/Input.vue"
import KeyBoard from "./components/main/Keyboard.vue"
import { ref } from 'vue'

const inputs = [ref(""), ref(""), ref("")]
var currentInput = 0

function type(letter) {
  inputs[currentInput].value += letter
}
function clear(idx) {
  console.log("cls");
  inputs[idx].value = ""
}
function newWord() {
  if (currentInput < 2) {
    currentInput++
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
        <KeyBoard @type="type" @valid="() => console.log('validation')" />
      </n-grid-item>
    </n-grid>
  </main>
</template>

<style scoped></style>

