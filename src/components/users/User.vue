<template>
    <div class="flex flex-col my-0.5 py-1.5 px-1.5 bg-zinc-700 cursor-pointer hover:bg-zinc-600">
        <p class="pointer-events-none">{{ userdata.chatname }}</p>
        <p class="text-zinc-400 whitespace-nowrap pointer-events-none">{{ date }} - {{ userdata.messages }} msgs</p>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps({
    userdata: {
        type: Object,
        default: {
            "chatname": "Name",
            "lastMessage": "Last message",
            "lastMessageTime": 0,
            "messages": 0,
        }
    },
})

// lastmessage time to relative time
const time = new Date(props.userdata.lastMessageTime * 1000)
const now = new Date()
const diff = now.getTime() - time.getTime()
const days = Math.floor(diff / (1000 * 60 * 60 * 24))
const hours = Math.floor(diff / (1000 * 60 * 60))
const minutes = Math.floor(diff / (1000 * 60))
const seconds = Math.floor(diff / 1000)
const relativeTime = days > 0 ? `${days}d` : hours > 0 ? `${hours}h` : minutes > 0 ? `${minutes}m` : `${seconds}s`

const date = ref(relativeTime)
</script>

<style scoped>
</style>