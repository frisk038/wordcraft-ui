<script setup>
import { NInput, NSpace, NIcon, NButton, NSpin, NStatistic, NNumberAnimation } from 'naive-ui'
import { CloseSharp, CheckmarkSharp } from '@vicons/ionicons5'
import { ref } from 'vue'

const emit = defineEmits(['clear', 'newWord', 'notExist'])
const props = defineProps(['word', 'isValidated'])
const score = ref(0)
const checking = ref(false)

async function checkAPI(wordToCheck) {
    let response = await fetch("https://wordcraft-397020.ew.r.appspot.com/pick/checkWord", {
        method: "POST",
        body: JSON.stringify({ word: wordToCheck })
    })
    if (response.ok) {
        return await response.json();
    } else {
        return Promise.reject(response);
    }
}

async function check() {
    if (props.word === "" || props.word.length < 2) return;

    checking.value = true
    let scoreRes = await checkAPI(props.word)
    if (scoreRes.exists === true) {
        score.value = scoreRes.score
        emit("newWord", score.value)
    }
    else {
        emit("notExist")
    }
    checking.value = false
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
