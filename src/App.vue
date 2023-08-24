<script setup>
import TitleBar from "./components/header/TitleBar.vue";
import { NDivider } from 'naive-ui'
import Input from "./components/main/Input.vue"
import KeyBoard from "./components/main/Keyboard.vue"
</script>

<template>
  <header>
    <TitleBar></TitleBar>
  </header>
  <main>
    <Input />
    <Input />
    <Input />
    <KeyBoard @typed="(n) => console.log(n)" @valid="() => console.log('validation')"></KeyBoard>
  </main>
</template>

<style scoped></style>

