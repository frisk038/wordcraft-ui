<script setup>
import { useMessage, NIcon } from 'naive-ui'
import { AlertCircleOutline, WarningOutline, InformationCircleOutline } from '@vicons/ionicons5'
import { h } from 'vue'


defineExpose({ createMessage })

const message = useMessage()

function createMessage(msg, type) {
    switch (type) {
        case "error":
            message.error(msg, {
                icon: () => h(NIcon, null, { default: () => h(AlertCircleOutline) }),
                duration: 1000
            })
            break;
        case "warning":
            message.warning(msg, {
                icon: () => h(NIcon, null, { default: () => h(WarningOutline) }),
                duration: 1000
            })
            break;
        case "info":
            message.info(msg, {
                icon: () => h(NIcon, null, { default: () => h(InformationCircleOutline) }),
                duration: 1000
            })
            break;
        default:
            break;
    }
}
</script>

<template></template>