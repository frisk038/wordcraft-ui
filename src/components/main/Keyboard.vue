<script setup>
import { ref } from 'vue'
import { NButton, NIcon, NGrid, NGridItem, NSpace } from 'naive-ui'
import { CloseSharp, CheckmarkSharp } from '@vicons/ionicons5'

defineEmits(['type', 'valid'])

const letters = ref(["a", "z", "e", "r", "t", "y", "i", "o", "p"])
const firstRow = letters.value.slice(0, 5)
const secondRow = letters.value.slice(5)


function getSvgPath(item) {
    return `./components/letters_svg/${item}.svg#layer1`
}
</script>

<template>
    <n-space justify="center" align="center" :wrap="true" size="small" :vertical="true">
        <n-grid :cols="5" x-gap="15">
            <n-grid-item v-for="(item, index) in firstRow" :key="index">
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

        <n-grid :cols="4" x-gap="15">
            <n-grid-item v-for="(item, index) in secondRow" :key="index">
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

        <n-button @click="$emit('valid')" circle>
            <template #icon>
                <n-icon>
                    <CheckmarkSharp />
                </n-icon>
            </template>
        </n-button>
    </n-space>
</template>

  