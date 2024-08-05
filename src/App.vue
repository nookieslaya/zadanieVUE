<template>
  <div class="h-[90%]">
    <div class="container mx-auto flex flex-wrap justify-center py-10">
      <Post v-for="post in paginatedPosts" :key="post.id" :post="post"/>
    </div>
    <div class="inline-flex justify-center items-center mx-auto w-full pb-10 gap-5">
      <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-l" @click="previousPage"
              :disabled="currentPage === 1">Poprzednia
      </button>
      <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-l" @click="nextPage"
              :disabled="currentPage === totalPages">Następna
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import {ref, onMounted, computed} from 'vue'
import Post from "./components/Post.vue";

const post = ref([])
const currentPage = ref(1)
const itemsPerPage = 10

const paginatedPosts = computed(() => {
  const startIndex = (currentPage.value - 1) * itemsPerPage
  return post.value.slice(startIndex, startIndex + itemsPerPage)
})

const totalPages = computed(() => Math.ceil(post.value.length / itemsPerPage))

onMounted(async () => {
  const response = await fetch('https://jsonplaceholder.typicode.com/posts')
  const json = await response.json()
  post.value = json
  console.log(json)
})

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
  }
}

const previousPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--
  }
}
</script>