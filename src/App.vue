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
    <KeyBoard></KeyBoard>
  </main>
</template>

<style scoped></style>

