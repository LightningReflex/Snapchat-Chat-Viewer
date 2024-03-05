<template>
    <!-- <div class="flex flex-col">
        <h1 class="text-2xl font-bold mb-5 px-2">Chats</h1>
        <User v-for="chat in chatList" :userdata="chat" :key="chat.chatname" />
    </div> -->

    <!-- same as above but scrollable -->
    <div class="flex flex-col max-h-full">
        <h1 class="text-2xl font-bold mb-5 px-2">Users</h1>
        <div class="overflow-y-auto">
            <User v-for="chat in chatList" :userdata="chat" :key="chat.chatname" @click="$emit('switchChat', chat.chatname)" />
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import User from './User.vue'

const props = defineProps<{
    data: Object
}>()

// console.log(props.data)

// raw chat data to chat list data
const chatList: any = ref([])

for (const [key, value] of Object.entries(props.data)) {
    // last message is end of the list
    const lastMessage = value[value.length - 1]
    chatList.value.push({
        chatname: key,
        lastMessage: lastMessage.Content,
        lastMessageTime: parseInt(lastMessage['Created(microseconds)'])/1000,
        messages: value.length
    })
}

</script>