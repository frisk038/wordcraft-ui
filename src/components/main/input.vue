<script setup>
import { NInput, NSpace, NIcon, NButton, NCard, NSpin, NStatistic, NNumberAnimation, NLayout } from 'naive-ui'
import { CloseSharp, CheckmarkSharp } from '@vicons/ionicons5'
import { ref } from 'vue'

const emit = defineEmits(['clear', 'newWord'])
const props = defineProps(['word'])

const score = ref(10)
const checking = ref(false)
const isValidated = ref(false)

function check() {
    if (props.word === "" || props.word.length < 2) return;

    checking.value = true
    console.log("checking if " + props.word + " is valid")
    setTimeout(() => {
        checking.value = false
        isValidated.value = true
    }, 2000);
    emit("newWord")
}
</script >
<template>
    <n-spin :show="checking" size="small">
        <n-space justify="center" align="center">
            <n-statistic label="Score">
                <n-number-animation :active="isValidated" :from="0" :to="score" />
            </n-statistic>

            <n-input type="text" size="large" placeholder="votre mot" round v-model:value="props.word" readonly />

            <n-button :disabled="isValidated" circle @click="$emit('clear')">
                <template #icon>
                    <n-icon>
                        <CloseSharp />
                    </n-icon>
                </template>
            </n-button>

            <n-button @click="check" :disabled="isValidated" circle>
                <template #icon>
                    <n-icon>
                        <CheckmarkSharp />
                    </n-icon>
                </template>
            </n-button>

        </n-space>

    </n-spin>
</template>
<script>


</script>
<style></style>