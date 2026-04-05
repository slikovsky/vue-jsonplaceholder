<script setup lang="ts">
import { ref, watch } from 'vue'
import BaseSelect from './BaseSelect.vue'

type Post = {
  id: number
  title: string
  userId: number
}

const props = defineProps<{
  userId: number
}>()
const model = defineModel<Post | null>()
const posts = ref<Post[]>([])

async function fetchPosts(userId: number) {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/posts?userId=${userId}`
  )
  posts.value = await res.json()
}

watch(
  () => props.userId,
  (newUserId) => {
    if (newUserId) {
      fetchPosts(newUserId)
      model.value = null // сброс выбора
    }
  },
  { immediate: true }
)
</script>

<template>
  <BaseSelect v-model="model" :options="posts" label="Посты">
    <template #default="{ option }">
      {{ option.title }}
    </template>
  </BaseSelect>
</template>
