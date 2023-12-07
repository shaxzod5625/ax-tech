<template>
  <div>
    <h1 class="text-4xl font-bold text-center">Posts</h1>
    <div class="flex justify-center my-4">
      <input
        type="text"
        class="border-2 border-gray-300 bg-white h-10 px-5 pr-16 rounded-lg text-sm focus:outline-none"
        placeholder="Search"
        v-model="search"
      />
      <select
        class="border-2 border-gray-300 bg-white h-10 px-5 pr-16 rounded-lg text-sm focus:outline-none"
        v-model="sort"
      >
        <option value="" selected>Sort by</option>
        <option value="title">Title</option>
        <option value="body">Body</option>
      </select>
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
    <div v-else class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">
      <NuxtLink v-for="post in posts" :key="post.id" :to="`/post/${post.id}`">
        <Post :post="post" />
      </NuxtLink>
    </div>
    <Pagination :total="total" :page="page" :perPage="perPage" @paginate="paginate" />
  </div>
</template>
<script setup lang="ts">
import {onMounted, ref, watch} from 'vue'

const posts = ref([])
const loading = ref(false)
const search = ref('')
const total = ref(0)
const perPage = ref(10)
const page = ref(1)
const sort = ref('')

const fetchPosts = async () => {
  loading.value = true
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/posts?_sort=${sort.value}&_order=desc&_page=${page.value}&_limit=${perPage.value}`
  )
  total.value = Number(response.headers.get('X-Total-Count'))
  posts.value = await response.json()
  loading.value = false
}

const searchPosts = async () => {
  loading.value = true
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/posts?q=${search.value}`
  )
  posts.value = await response.json()
  loading.value = false
}

watch(search, (value: string) => {
  if (value.length < 1) {
    fetchPosts()
    return
  }
  const timer = setTimeout(() => {
    searchPosts()
  }, 300)
  return () => clearTimeout(timer)
})

watch(sort, (value: string) => {
  if (value.length < 1) {
    fetchPosts()
    return
  }
  const timer = setTimeout(() => {
    fetchPosts()
  }, 300)
  return () => clearTimeout(timer)
})

const paginate = (newPage: number) => {
  page.value = newPage
  fetchPosts()
}

onMounted(async () => {
  await fetchPosts()
})
</script>

<!--
Можно было все запросы запихнуть в api папку server/posts.ts и т.д.
Но я решил сделать все в одном файле для простоты и удобства
-->