<script setup>
import { ref } from 'vue'
import { NButton, NIcon, NGrid, NGridItem } from 'naive-ui'
import { CloseSharp, CheckmarkSharp } from '@vicons/ionicons5'

defineEmits(['type', 'valid'])

const letters = ref(["a", "z", "e", "r", "t", "y", "i", "o", "p", "q", "s", "d", "f", "g", "h"]);

function getSvgPath(item) {
    return `src/components/letters_svg/${item}.svg#layer1`;
}

</script >

<template >
    <n-grid x-gap="5" :cols="3">
        <n-grid-item>
            <n-button v-for="(item, index) in letters" :key="index" @click="$emit('type', item)" circle>
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
                <CheckmarkSharp></CheckmarkSharp>
            </n-icon>
        </template>
    </n-button>
</template>

<script>
</script>
<style ></style>