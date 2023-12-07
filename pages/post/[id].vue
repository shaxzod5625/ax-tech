<template>
  <div class="container mx-auto px-4">
    <div class="flex justify-center">
      <div class="w-full md:w-2/3">
        <div class="flex justify-between items-center">
          <h1 class="text-3xl font-bold text-gray-800">Post</h1>
          <NuxtLink class="text-gray-800" to="/">Back</NuxtLink>
        </div>
        <div class="mt-4">
          <h2 class="text-xl font-bold text-gray-800">{{ post.title }}</h2>
          <p class="mt-2 text-gray-600">{{ post.body }}</p>
        </div>
        <div class="mt-4">
          <h2 class="text-xl font-bold text-gray-800">Comments</h2>
          <div class="mt-2">
            <input
              class="w-full px-4 py-2 text-gray-700 bg-gray-200 rounded"
              type="text"
              placeholder="Search comments..."
              v-model="search"
            />
          </div>
          <div v-if="loading" class="flex justify-center my-4">
            <svg
                class="animate-spin -ml-1 mr-3 h-5 w-5 text-gray-900"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
            >
              <circle
                  class="opacity-25"
                  cx="12"
                  cy="12"
                  r="10"
                  stroke="currentColor"
                  stroke-width="4"
              ></circle>
              <path
                  class="opacity-75"
                  fill="currentColor"
                  d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
              ></path>
            </svg>
          </div>
          <div v-else class="mt-2">
            <InfiniteScroll :on-infinite="fetchComments" :distance="1">
              <div v-for="comment in comments" :key="comment.id">
                <Comment :comment="comment" />
              </div>
            </InfiniteScroll>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'

const route = useRoute()
const { id } = route.params

const post = ref({})
const loading = ref(false)
const fetchPost = async () => {
  loading.value = true
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`)
  post.value = await response.json()
  loading.value = false
}

const comments = ref([])
const search = ref('')
const fetchComments = async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/comments?postId=${id}`
  )
  comments.value = await response.json()
}

const searchComments = async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/comments?postId=${id}&q=${search.value}`
  )
  comments.value = await response.json()
}

watch(search, (value: string) => {
  if (value.length < 1) {
    fetchComments()
    return
  }
  setTimeout(() => {
    searchComments()
  }, 300);
})

onMounted(async () => {
  await fetchPost()
  await fetchComments()
})
</script>