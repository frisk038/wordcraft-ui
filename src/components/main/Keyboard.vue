<script setup>
import { ref } from 'vue'
import { NButton, NIcon } from 'naive-ui'
import { CloseSharp } from '@vicons/ionicons5'

const letters = ref(["a", "z", "e", "r", "t", "y", "i", "o", "p", "q", "s", "d", "f", "g", "h",
    "j", "k", "l", "m", "w", "x", "c", "v", "b", "n"]);

function getSvgPath(item) {
    return `src/components/letters_svg/${item}.svg#layer1`;
}

</script >

<template >
    <n-button v-for="(item, index) in letters" :key="index" circle>
        <template #icon>
            <n-icon>
                <svg viewBox="0 0 111 111">
                    <use :xlink:href="getSvgPath(item)" />
                </svg>
            </n-icon>
        </template>
    </n-button>
</template>

<script>
export default {


}
</script>
<style ></style>