<template>
  <div v-if="!deleted" class="p-4 max-w-sm h-full flex flex-col">
    <div class="flex justify-evenly rounded-lg h-full dark:bg-gray-800 bg-teal-400 p-8 flex-col">
      <div class="flex flex-col h-full min-h-[200px] justify-evenly">
        <h2 class="text-white dark:text-white text-lg font-medium mb-3">{{ post.title }}</h2>

        <p class="leading-relaxed text-base text-white dark:text-gray-300 mt-3" v-if="showMore">{{ post.body }}</p>
        <p class="leading-relaxed text-base text-white dark:text-gray-300" v-else>{{ shortenedText }}...</p>

        <button class="mt-3 text-black dark:text-white hover:text-blue-600 inline-flex items-center"
                @click="toggleShowMore">
          {{ showMore ? 'Pokaż mniej' : 'Pokaż więcej' }}
        </button>

        <p>{{ post.author }}</p>
      </div>
      <button
          class=" bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow mt-4"
          @click="deletePost(post.id)">Usuń news
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import {ref, computed} from 'vue'
import {toRefs} from 'vue'

const props = defineProps(['post'])

const {post} = toRefs(props)

const deleted = ref(false)
const showMore = ref(false)
const shortenedText = computed(() => post.value.body.slice(0, 70))
const toggleShowMore = () => {
  showMore.value = !showMore.value
}

const deletePost = async (id) => {
  try {
    await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
      method: 'DELETE',
    })

    deleted.value = true
  } catch (error) {
    console.error('Error deleting post:', error)
  }
}
</script>

