<script setup lang="ts">
import { ref, onMounted } from 'vue'
import BaseSelect from './BaseSelect.vue'

type User = {
  id: number
  name: string
}

const model = defineModel<User | null>()
const users = ref<User[]>([])

onMounted(async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users')
  users.value = await res.json()
})
</script>

<template>
  <BaseSelect v-model="model" :options="users" label="Пользователи" >
    <template #default="{ option }">
      {{ option.name }}
    </template>
  </BaseSelect>
</template>
