<script setup>
import { NCard, NModal, NText, NButton, NSpace, NIcon, NInput } from 'naive-ui'
import { ClipboardOutline, SaveOutline } from '@vicons/ionicons5'
import { ref } from "vue";

const emit = defineEmits(['newUser', 'copy'])
const props = defineProps(['userID'])
const userInput = ref('')

// Cookie helper
function setCookieAndExpire(cname, cvalue, expire) {
    document.cookie = cname + "=" + cvalue + ";expires=" + expire.toUTCString();
}

async function registerUser() {
    let api = await fetch("https://wordcraft-397020.ew.r.appspot.com/user/register", {
        method: "POST",
        body: JSON.stringify({
            name: userInput.value.toUpperCase()
        })
    })
    if (api.ok) {
        const resp = await api.json()
        setCookieAndExpire("userid", resp.id, new Date("2030/01/01"))
        emit("newUser")
    }
    else {
        return Promise.reject(response);
    }
}
</script>

<template>
    <n-modal preset="card" title="👏🏾 Bravo" style="width: 65%" :z-index="100">
        <n-card :bordered="false" size="small">
            <n-space :vertical="true" align="center" v-if="userID">
                <n-text depth="1">
                    Vos <n-text type="info">mots</n-text> ont été pris en compte ! <br>
                    Vous pouvez cliquer sur le bouton 📋 ci-dessous pour copier et partager votre score ! <br>
                    Vous pouvez aussi voir votre classement en cliquant sur le bouton 📊 en haut a gauche.
                </n-text>
                <n-button circle size="large" @click="$emit('copy')">
                    <template #icon>
                        <n-icon>
                            <ClipboardOutline />
                        </n-icon>
                    </template>
                </n-button>
            </n-space>

            <n-space :vertical="true" align="center" v-else>
                <n-text depth="1">
                    Enregistre ton score 📝, et pose ta place dans le classement en entrant un pseudo 🏆
                </n-text>
                <n-space align="center">
                    <n-input maxlength="5" show-count placeholder="un pseudo sympa" v-model:value="userInput"
                        round></n-input>
                    <n-button circle size="large" @click="registerUser">
                        <template #icon>
                            <n-icon>
                                <SaveOutline />
                            </n-icon>
                        </template>
                    </n-button>
                </n-space>
            </n-space>
        </n-card>
    </n-modal>
</template>
