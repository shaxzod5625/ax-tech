<template>
  <div>
    <slot></slot>
  </div>
</template>
<script setup lang="ts">
import { defineProps, onMounted, ref, watch } from 'vue'
const emit = defineEmits(['on-infinite'])
defineProps<{
  distance: number
}>()

const scrollPosition = ref(0)

const handleScroll = () => {
  const scrollHeight = document.documentElement.scrollHeight
  const scrollTop = document.documentElement.scrollTop
  const clientHeight = document.documentElement.clientHeight
  const scrolledToBottom = Math.ceil(scrollTop + clientHeight) >= scrollHeight
  if (scrolledToBottom) {
    emit('on-infinite')
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

watch(scrollPosition, (value) => {
  console.log(value)
})
</script>