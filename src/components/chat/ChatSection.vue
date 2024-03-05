<template>
    <div class="mb-2">
        <p class="font-bold">{{ section[0] }}</p>
        <ChatLine v-for="message in section.filter((message) => typeof message !== 'string')" :message="message"/>
    </div>
</template>

<script setup lang="ts">
import ChatLine from './ChatLine.vue'
defineProps<{ section: Array<any> }>()
</script>