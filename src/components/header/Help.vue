<script setup>
import { NCard, NModal, NText } from 'naive-ui'
</script>

<template>
    <n-modal preset="dialog" title="Règles du jeu" size="small" aria-modal="true">
        <n-card :bordered="false">
            <n-text depth="1">
                Tous les jours, composez <n-text type="info">trois mots</n-text> avec un nombre de lettres définies.
                Les lettres sont tirées au hasard essayez de composer <n-text type="success">le mot le plus long</n-text> !
            </n-text>
            <template #footer>
                Bonne chance !
            </template>
        </n-card>
    </n-modal>
</template>
<script>
export default {

}
</script>