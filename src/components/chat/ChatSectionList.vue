<template>
    <div v-for="chatSection in formattedChatData"  :class="{'hover:bg-blue-950': !chatSection[1].IsSender, 'hover:bg-red-950': chatSection[1].IsSender}">
        <!-- <p>{{ chatSection[0] }}</p>
        <p v-for="message in chatSection" v-if="!(typeof message == 'string')">{{ message }}</p> -->
        <ChatSection :section="chatSection"/>
    </div>
</template>

<script setup lang="ts">
import ChatSection from './ChatSection.vue'
defineProps<{ formattedChatData: any }>()
</script>