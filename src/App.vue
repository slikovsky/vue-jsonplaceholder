<script setup lang="ts">
import { ref, watch } from 'vue'
import UserSelect from './components/UserSelect.vue'
import PostSelect from './components/PostSelect.vue'
import CommentSelect from './components/CommentSelect.vue'

type User = {
  id: number
  name: string
}
type Post = {
  id: number
  title: string
  userId: number
}
type Comment = {
  id: number
  email: string
  body: string
  postId: number
}

const selectedUser = ref<User | null>(null)
const selectedPost = ref<Post | null>(null)
const selectedComment = ref<Comment | null>(null)

watch(selectedUser, () => {
  selectedPost.value = null
  selectedComment.value = null
})
watch(selectedPost, () => {
  selectedComment.value = null
})
</script>

<template>
  <div class="container">
    <div class="card">
      <h2>Выберите данные</h2>
      <UserSelect v-model="selectedUser" />
      <PostSelect v-if="selectedUser" v-model="selectedPost" :userId="selectedUser?.id" />
      <CommentSelect v-if="selectedPost" v-model="selectedComment" :postId="selectedPost?.id" />
    </div>
    <div class="card">
      <h2>Выбранный комментарий</h2>
      <div v-if="selectedComment">
        <p><b>Автор:</b> {{ selectedComment.email }}</p>
        <p>{{ selectedComment.body }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  background: linear-gradient(135deg, #800000, #696969, #000000);
  color: white;
  box-sizing: border-box;
}

.card {
  background: linear-gradient(135deg, #000000, #696969, #800000);
  padding: 20px;
  margin: 10px;
  border-radius: 16px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.6);
  flex: 1;
  width: 400px;
  max-width: 100%;
}
</style>
