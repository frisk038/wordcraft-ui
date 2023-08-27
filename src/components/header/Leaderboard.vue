<script setup>
import { NCard, NModal, NSpace, NDataTable } from 'naive-ui'
import { ClipboardOutline } from '@vicons/ionicons5'
import { ref } from "vue";

const props = defineProps(['gameID'])

const data = [
    {
        key: 0,
        name: 'John Brown',
        score: 32,
    },
    {
        key: 1,
        name: 'Jim Green',
        score: 42,
    },
    {
        key: 2,
        name: 'Joe Black',
        score: 32,
    }
]

const columns = [
    {
        title: "Joueur",
        key: "name"
    },
    {
        title: "Score",
        key: "score"
    }
]

async function getLeaderBoard() {
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
    <n-modal preset="card" title="🏆 Classement" style="width: 65%">
        <n-card :bordered="false" size="small">
            <n-data-table :columns="columns" :data="data" />
        </n-card>
    </n-modal>
</template>
