<template>
    <div>
        <!-- <div class="border-l-4 pl-1" :class="{'border-blue-400': !message.IsSender, 'border-red-400': message.IsSender}">
            <p class="text-wrap">{{ message.Content }}</p>
        </div> -->
        <!-- right side send time -->
        <div class="flex justify-between" :class="{'hover:bg-blue-900': !message.IsSender, 'hover:bg-red-900': message.IsSender}">
            <div class="border-l-4 pl-1" :class="{'border-blue-400': !message.IsSender, 'border-red-400': message.IsSender}">
                <!-- <p class="text-wrap">{{ message.Content }}</p> -->
                    <!-- "Media Type": "(?!TEXT"|MEDIA"|STICKER"|NOTE"|NONPARTICIPANTBOTRESPONSE"|STATUS"|SHARE"|STATUSCONVERSATIONCAPTURESCREENSHOT"|STATUSERASEDMESSAGE"|STATUSSAVETOCAMERAROLL"|STATUSCALLMISSEDVIDEO") -->
                    <template v-if="message['Media Type'] === 'TEXT'">
                        <p class="text-wrap">{{ message.Content }}</p>
                    </template>
                    <template v-else-if="message['Media Type'] === 'MEDIA'">
                        <!-- snapchat doesn't provide image link or redirect so just like put a placeholder image -->
                        <div class="flex flex-row">
                            <img src="https://via.placeholder.com/150" alt="media" class="w-20 h-20 pr-2" />
                            <div class="flex flex-col justify-center">
                                <p class="text-sm text-gray-400">MEDIA (UNSUPPORTED)</p>
                            </div>
                        </div>
                    </template>
                    <template v-else-if="message['Media Type'] === 'STICKER'">
                        <div class="flex flex-row">
                            <img src="https://via.placeholder.com/150" alt="sticker" class="w-20 h-20 pr-2" />
                            <div class="flex flex-col justify-center">
                                <p class="text-sm text-gray-400">STICKER (UNSUPPORTED)</p>
                            </div>
                        </div>
                    </template>
                    <template v-else-if="message['Media Type'] === 'NOTE'">
                        <p class="text-sm text-gray-400">NOTE (I THINK THIS IS A VOICE NOTE) (UNSUPPORTED)</p>
                    </template>
                    <template v-else-if="message['Media Type'] === 'NONPARTICIPANTBOTRESPONSE'">
                        <!-- same as normal but bot -->
                        <div class="flex flex-row">
                            <p class="text-wrap pr-2">{{ message.Content }}</p>
                            <div class="flex flex-col justify-center">
                                <p class="text-sm text-gray-400">BOT</p>
                            </div>
                        </div>
                    </template>
                    <template v-else-if="message['Media Type'] === 'STATUS'">
                        <p class="text-sm text-gray-400">STATUS (UNSUPPORTED)</p>
                    </template>
                    <template v-else-if="message['Media Type'] === 'SHARE'">
                        <p class="text-sm text-gray-400">Shared a video or something (SHARE)</p>
                    </template>
                    <template v-else-if="message['Media Type'] === 'STATUSCONVERSATIONCAPTURESCREENSHOT'">
                        <p class="text-sm text-gray-400">Screenshot taken of chat (or call idk) (STATUSCONVERSATIONCAPTURESCREENSHOT)</p>
                    </template>
                    <template v-else-if="message['Media Type'] === 'STATUSERASEDMESSAGE'">
                        <p class="text-sm text-gray-400">Message deleted (STATUSERASEDMESSAGE)</p>
                    </template>
                    <template v-else-if="message['Media Type'] === 'STATUSSAVETOCAMERAROLL'">
                        <p class="text-sm text-gray-400">Image saved to camera roll (STATUSSAVETOCAMERAROLL)</p>
                    </template>
                    <template v-else-if="message['Media Type'] === 'STATUSCALLMISSEDVIDEO'">
                        <p class="text-sm text-gray-400">Missed video call (STATUSCALLMISSEDVIDEO)</p>
                    </template>
            </div>
            <!-- <p class="text-xs text-gray-400 text-nowrap pl-2">{{ message.Created }}</p> -->
            <!-- format it propperly message["Created(microseconds)"] -->
            <p class="text-xs text-gray-400 text-nowrap pl-2">{{ formattedTime }}</p>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const props = defineProps<{ message: any }>()

// format dd/mm/yyyy hh:mm:ss
const time = new Date(props.message['Created(microseconds)'])
// const formattedTime = ref(`${time.getDate()}/${time.getMonth()}/${time.getFullYear()} ${time.getHours()}:${time.getMinutes()}:${time.getSeconds()}`)
// add zero's infront of single digit numbers

const formattedTime = ref(`${time.getDate().toString().padStart(2, '0')}/${(time.getMonth() + 1).toString().padStart(2, '0')}/${time.getFullYear()} ${time.getHours().toString().padStart(2, '0')}:${time.getMinutes().toString().padStart(2, '0')}:${time.getSeconds().toString().padStart(2, '0')}`)
</script>