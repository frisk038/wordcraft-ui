<script setup>
import { NButton, NH1, NSpace, NIcon, NCard, NModal } from 'naive-ui'
import { StatsChartOutline, HelpSharp } from '@vicons/ionicons5'
import { ref } from "vue";

import Help from './Help.vue'
</script>

<template>
    <NSpace justify="space-between" align="center">
        <n-button round @click="showModal = true">
            <n-icon>
                <StatsChartOutline />
            </n-icon>
        </n-button>
        <n-h1>WordCraft</n-h1>
        <n-button round @click="showModal = true">
            <n-icon>
                <HelpSharp />
            </n-icon>
        </n-button>

        <Help v-model:show="showModal"></Help>

    </NSpace>
</template>

<script>
const showModal = ref(false)
</script>

<style></style>