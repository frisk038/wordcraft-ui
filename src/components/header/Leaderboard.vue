<script setup>
import { NCard, NModal, NSpace, NDataTable } from 'naive-ui'
import { ClipboardOutline } from '@vicons/ionicons5'
import { ref, onMounted } from "vue";

const props = defineProps(['gameID'])

const data = ref([])

let b = [
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
    let api = await fetch("https://wordcraft-397020.ew.r.appspot.com/user/leaderboard")
    if (api.ok) {
        const resp = await api.json()
        data.value = resp
    }
    else {
        return Promise.reject(response);
    }
}


getLeaderBoard()
</script>

<template>
    <n-modal preset="card" title="🏆 Classement" style="width: 65%">
        <n-card :bordered="false" size="small">
            <n-data-table :columns="columns" :data="data" />
        </n-card>
    </n-modal>
</template>
