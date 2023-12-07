<template>
  <div class="mt-4">
    <div class="flex justify-between items-center">
      <h3 class="text-lg font-bold text-gray-800">{{ comment.name }}</h3>
      <span class="text-sm text-gray-600">{{ comment.email }}</span>
    </div>
    <p class="mt-2 text-gray-600">{{ comment.body }}</p>
  </div>
</template>
<script setup lang="ts">
import { defineProps } from 'vue'

defineProps<{
  comment: {
    postId: number
    id: number
    name: string
    email: string
    body: string
  }
}>()
</script>