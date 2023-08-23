<script setup>
import { NInput, NSpace, NIcon, NButton, NCard } from 'naive-ui'
import { CloseSharp } from '@vicons/ionicons5'
import { ref } from "vue";

const word = ref("ff")
function clear() {
    word.value = "";
}

</script >

<template>
    <n-space justify="center" align="center">
        <n-input type="text" size="large" placeholder="votre bafouille" round v-model:value="word" readonly />
        <n-button circle @click="clear">
            <template #icon>
                <n-icon>
                    <CloseSharp />
                </n-icon>
            </template>
        </n-button>
    </n-space>
</template>

<script>


</script>
<style></style>