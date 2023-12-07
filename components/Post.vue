<template>
  <div>
    <div class="bg-white shadow-md rounded-md p-4">
      <h2 class="text-lg font-semibold text-gray-800 capitalize truncate">
        {{ post.title }}
      </h2>
      <p class="text-gray-600 mt-2">{{ post.body }}</p>
    </div>
  </div>
</template>
<script setup lang="ts">
import { defineProps } from 'vue'

interface Post {
  userId: number,
  id: number,
  title: string,
  body: string
}

defineProps<{
  post: Post
}>()
</script>