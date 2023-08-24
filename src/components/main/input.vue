<script setup>
import { NInput, NSpace, NIcon, NButton, NCard } from 'naive-ui'
import { CloseSharp, CheckmarkSharp } from '@vicons/ionicons5'

const emit = defineEmits(['clear', 'newWord'])
const props = defineProps(['word'])

function check() {
    console.log("check api")
    emit("newWord")
}
</script >
<template>
    <n-space justify="center" align="center">
        <n-input type="text" size="large" placeholder="votre bafouille" round v-model:value="props.word" readonly />
        <n-button circle @click="$emit('clear')">
            <template #icon>
                <n-icon>
                    <CloseSharp />
                </n-icon>
            </template>
        </n-button>
        <n-button @click="check" circle>
            <template #icon>
                <n-icon>
                    <CheckmarkSharp />
                </n-icon>
            </template>
        </n-button>
    </n-space>
</template>
<script>


</script>
<style></style>