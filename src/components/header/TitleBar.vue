<script setup>
import { NButton, NH1, NSpace, NIcon, NGradientText } from 'naive-ui'
import { StatsChartOutline, HelpSharp } from '@vicons/ionicons5'
import { ref } from "vue";

import Help from './Help.vue'
</script>

<template>
    <NSpace justify="space-evenly" align="center">
        <n-button circle @click="showModal = true">
            <template #icon>
                <n-icon>
                    <StatsChartOutline />
                </n-icon>
            </template>
        </n-button>

        <n-gradient-text :gradient="{
            from: '#7209b7',
            to: '#480ca8'
        }" :font-size="32">
            WordCraft
        </n-gradient-text>

        <n-button circle @click="showModal = true">
            <template #icon>
                <n-icon>
                    <HelpSharp />
                </n-icon>
            </template>
        </n-button>



    </NSpace>
    <Help v-model:show="showModal"></Help>
</template>

<script>
const showModal = ref(false)
</script>

<style></style>