<script setup>
import { NButton, NBadge, NSpace, NIcon, NGradientText } from 'naive-ui'
import { StatsChartOutline, HelpSharp } from '@vicons/ionicons5'
import { ref } from "vue";
import Help from './Help.vue'
import LeaderBoard from './Leaderboard.vue'

const showHelp = ref(false)
const showLead = ref(false)
</script>

<template>
    <NSpace justify="space-evenly" align="center">
        <n-badge dot processing>
            <n-button circle @click="showLead = true">
                <template #icon>
                    <n-icon>
                        <StatsChartOutline />
                    </n-icon>
                </template>
            </n-button>
        </n-badge>
        <n-gradient-text :gradient="{
            from: '#7209b7',
            to: '#480ca8'
        }" :font-size="32">
            WordCraft
        </n-gradient-text>

        <n-button circle @click="showHelp = true">
            <template #icon>
                <n-icon>
                    <HelpSharp />
                </n-icon>
            </template>
        </n-button>
    </NSpace>

    <Help v-model:show="showHelp"></Help>
    <LeaderBoard v-model:show="showLead"></LeaderBoard>
</template>

<script>
</script>

<style></style>