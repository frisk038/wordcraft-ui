<script setup>
import TitleBar from "./components/header/TitleBar.vue";
import { NDivider } from 'naive-ui'
import Input from "./components/main/Input.vue"
</script>

<template>
  <header>
    <TitleBar></TitleBar>
  </header>
  <main>
    <Input />
    <Input />
    <Input />
  </main>
</template>

<style scoped></style>
