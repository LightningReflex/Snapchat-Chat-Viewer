<template>
    <!-- upload stuff -->
    <div v-if="data === ''" class="h-screen dark:bg-zinc-800 flex flex-col justify-center items-center">
        <h1 class="dark:text-white text-5xl text-center py-10 font-bold">Chat Analyzer</h1>
        <div class="w-64 md:w-80 lg:w-96 h-80">
            <div>
                <div class="text-center p-5 border-2 border-dashed border-black rounded-lg transition" @dragover.prevent @drop="onDrop" @dragenter="onDragEnter" @dragleave="onDragLeave" :class="{ 'bg-gray-500 dark:bg-gray-800': isDragging, 'bg-gray-300 dark:bg-gray-900': !isDragging }">
                    <p class="pointer-events-none dark:text-white">{{ isDragging ? "Release to load chat" : "Drag chat_history.json to load chat" }}</p>
                </div>
            </div>
        </div>
    </div>


    <!-- stuff uploaded -->
    <div v-else class="h-screen dark:bg-zinc-800 flex flex-col items-center">
        <!-- navbar -->
        <nav class="flex justify-between items-center w-full h-16 p-5 bg-gray-300 dark:bg-zinc-800">
            <h1 class="dark:text-white text-3xl text-center font-bold">Chat Analyzer</h1>
            <div class="flex justify-center items-center">
                <button class="bg-gray-500 dark:bg-zinc-400 text-white dark:text-black px-5 py-2 rounded-lg hover:bg-zinc-200 transition" @click="data = ''">Back</button>
            </div>
        </nav>

        <!-- main content -->
        <div class="flex flex-row w-full h-[calc(100%_-_4rem)] justify-between">
            <div class="dark:bg-zinc-900 max-w-80 mr-6 pt-5 rounded-tr-xl text-white">
                <!-- people -->
                <UsersList :data="data" @switchChat="switchChat"/>
            </div>

            <div class="dark:bg-zinc-900 w-full pt-5 rounded-tl-xl text-white">
                <!-- text -->
                <div class="flex flex-col h-full">
                    <h1 class="text-2xl font-bold mb-5 px-2">Chat</h1>
                    <!-- <p class="px-2">a</p> -->
                    <!-- loop currentChat and add -->
                    <!-- <div v-if="currentChat !== ''">
                        <p>TEXT</p>
                        <div v-for="chat in formattedData[currentChat]">
                            <p>{{ chat[0] }}</p>
                            <p v-for="message in chat" v-if="!(typeof message == 'string')">{{ message.Content }}</p>
                        </div>
                    </div> -->
                    <div class="overflow-y-auto" v-if="currentChat !== ''">
                        <ChatSectionList :formattedChatData="formattedData[currentChat]"/>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script setup lang="ts">
import { ref } from 'vue'
import UsersList from './components/users/UsersList.vue'
import ChatSectionList from './components/chat/ChatSectionList.vue'

// file drop
const data = ref('')
const formattedData: any = ref({})
const onDrop = (e: DragEvent) => {
    e.preventDefault()
    const file = e.dataTransfer?.files[0]
    if (file && file.type === 'application/json') {
        const reader = new FileReader()
        reader.onload = (e) => {
            const content = e.target?.result as string
            // data.value = content

            // parse json
            const json = JSON.parse(content)
            data.value = json

            // parse data into sections (group consecutive "From")

            // const formattedData: any = {}
            const formattedDataCode: any = {}
            for (const [chatid, value] of Object.entries(json)) { // for every chat log
                // reverse the array
                const messages = value as any[]
                messages.reverse()
                if (!formattedDataCode[chatid]) {
                    formattedDataCode[chatid] = []
                }

                // loop through the messages and list together the consecutive "From"
                let lastFrom = ''
                for (const message of messages) { // for every message in this log
                    if (message.From === lastFrom) {
                        // add to last message
                        const lastId = formattedDataCode[chatid].length - 1
                        // remove "From" from message
                        const { From, ...rest } = message
                        formattedDataCode[chatid][lastId].push(rest)
                    } else {
                        // add new message
                        formattedDataCode[chatid].push([])
                        const lastId = formattedDataCode[chatid].length - 1
                        formattedDataCode[chatid][lastId].push(message.From)
                        // remove "From" from message
                        const { From, ...rest } = message
                        formattedDataCode[chatid][lastId].push(rest)
                    }
                    lastFrom = message.From
                }
                // console.log(formattedData[chatid])
            }
            // console.log("below")
            // console.log(formattedData)
            formattedData.value = formattedDataCode
            console.log(formattedData.value)
        }
        reader.readAsText(file)
    }
}


const isDragging = ref(false)
const onDragEnter = () => {
    isDragging.value = true
}
const onDragLeave = () => {
    isDragging.value = false
}


const currentChat = ref('')
// switch chat
const switchChat = (chat: any) => {
    // console.log(chat)
    currentChat.value = chat
}
</script>

<style scoped>
</style>
