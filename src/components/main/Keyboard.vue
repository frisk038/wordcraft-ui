<script setup>
import { ref } from 'vue'
import { NButton, NIcon, NGrid, NGridItem, NSpace } from 'naive-ui'
import { CloseSharp, CheckmarkSharp } from '@vicons/ionicons5'

defineEmits(['type', 'valid'])

const letters = ref([])
const gameID = ref("")
var firstRow = []
var secondRow = []

function getSvgPath(item) {
    return `assets/letters_svg/${item}.svg#layer1`
}

async function getLetters() {
    let response = await fetch("https://wordcraft-397020.ew.r.appspot.com/pick/getLetters")
    if (response.ok) {
        const js = await response.json();
        letters.value = js.letters
        gameID.value = js.id
        firstRow = letters.value.slice(0, 5)
        secondRow = letters.value.slice(5)
    } else {
        return Promise.reject(response);
    }
}

const res = await getLetters()
</script>

<template>
    <n-space justify="center" align="center" :wrap="true" size="small" :vertical="true">
        <n-grid :cols="5" x-gap="15">
            <n-grid-item v-for="(item, index) in firstRow" :key="index">
                <n-button @click="$emit('type', item)" circle>
                    <template #icon>
                        <n-icon>
                            <svg viewBox="0 0 111 111">
                                <use :href="getSvgPath(item)" />
                            </svg>
                        </n-icon>
                    </template>
                </n-button>
            </n-grid-item>
        </n-grid>

        <n-grid :cols="4" x-gap="15">
            <n-grid-item v-for="(item, index) in secondRow" :key="index" style="{ color: red }">
                <n-button @click="$emit('type', item)" circle>
                    <template #icon>
                        <n-icon>
                            <svg viewBox="0 0 111 111">
                                <use :xlink:href="getSvgPath(item)" />
                            </svg>
                        </n-icon>
                    </template>
                </n-button>
            </n-grid-item>
        </n-grid>

        <n-button @click="$emit('valid', gameID)" circle>
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