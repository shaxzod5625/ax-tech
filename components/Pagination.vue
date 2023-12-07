<template>
  <div class="flex justify-center my-4">
    <!--  prev btn  -->
    <button
      class="px-4 py-2 text-sm font-medium tracking-wide text-white capitalize transition-colors duration-200 transform bg-gray-800 rounded-md hover:bg-gray-600 focus:outline-none focus:bg-gray-600"
      :disabled="page === 1"
      @click="paginate(page - 1)"
    >
      Previous
    </button>
    <!--  show info about pages  -->
    <span class="mx-2 px-4 py-2 text-sm font-medium tracking-wide text-gray-700 capitalize bg-gray-200 rounded-md">
      Page {{ page }} of {{ totalPages }}
    </span>
    <!--  next btn  -->
    <button
      class="px-4 py-2 text-sm font-medium tracking-wide text-white capitalize transition-colors duration-200 transform bg-gray-800 rounded-md hover:bg-gray-600 focus:outline-none focus:bg-gray-600"
      :disabled="page === totalPages"
      @click="paginate(page + 1)"
    >
      Next
    </button>
  </div>
</template>
<script setup lang="ts">
import { defineProps, computed } from 'vue'

const emit = defineEmits(['paginate'])

const props = defineProps<{
  total: number
  perPage: number
  page: number
}>()

const totalPages = computed(() => Math.ceil(props.total / props.perPage))

const paginate = (newPage: number) => {
  emit('paginate', newPage)
}
</script>