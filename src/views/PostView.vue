<script setup lang="ts">
import axios from 'axios'
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import type { Post, PostComment } from '@/types/post'

const post = ref<Post | null>(null)
const comments = ref<PostComment[]>([])
const route = useRoute()

onMounted(async () => {
  const { data }: { data: Post } = await axios.get(
    `https://jsonplaceholder.typicode.com/posts/${route.params.id}`,
  )
  post.value = data

  const { data: commentsData }: { data: PostComment[] } = await axios.get(
    `https://jsonplaceholder.typicode.com/posts/${route.params.id}/comments`,
  )

  comments.value = commentsData
})
</script>

<template>
  <div class="post">
    <div v-if="post" class="card">
      <div class="card-body">
        <h1 class="card-title">{{ post.title }}</h1>
        <p class="card-text">{{ post.body }}</p>
      </div>
    </div>

    <div v-if="comments.length" class="mt-4">
      <h2>Comments</h2>
      <ul class="list-group">
        <li v-for="comment in comments" :key="comment.id" class="list-group-item">
          <h5>{{ comment.name }}</h5>
          <p>{{ comment.body }}</p>
          <small class="text-muted">By: {{ comment.email }}</small>
        </li>
      </ul>
    </div>
  </div>
</template>
