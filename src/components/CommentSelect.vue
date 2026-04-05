<script setup lang="ts">
import { ref, watch } from 'vue'
import BaseSelect from './BaseSelect.vue'

type Comment = {
  id: number
  email: string
  body: string
  postId: number
}

const props = defineProps<{
  postId: number
}>()
const model = defineModel<Comment | null>()
const comments = ref<Comment[]>([])

async function fetchComments(postId: number) {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/comments?postId=${postId}`
  )
  comments.value = await res.json()
}

watch(
  () => props.postId,
  (newPostId) => {
    if (newPostId) {
      fetchComments(newPostId)
      model.value = null // сброс
    }
  },
  { immediate: true }
)
</script>

<template>
  <BaseSelect v-model="model" :options="comments" label="Комментарии">
    <template #default="{ option }">
      {{ option.email }}
    </template>
  </BaseSelect>
</template>
