<script setup>
import { NButton, NH1, NSpace, NIcon, NGradientText } from 'naive-ui'
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

        <n-gradient-text :gradient="{
            from: '#7209b7',
            to: '#480ca8'
        }" :font-size="32">
            WordCraft
        </n-gradient-text>

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