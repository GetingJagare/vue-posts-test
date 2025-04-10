<script lang="ts" setup>
import axios from 'axios'
import { defineProps, ref, onMounted } from 'vue'
import type { IPost, IPostComment } from '@/types/post'
const props = defineProps<{ postId: number }>()
const comments = ref<IPostComment[]>([])

onMounted(async () => {
  const { data }: { data: IPostComment[] } = await axios.get(
    `https://jsonplaceholder.typicode.com/posts/${props.postId}/comments`,
  )
  comments.value = data
})
</script>

<template>
  <div class="mt-4" v-if="comments.length">
    <h2>Comments</h2>
    <ul class="list-group">
      <li v-for="comment in comments" :key="comment.id" class="list-group-item">
        <h5>{{ comment.name }}</h5>
        <p>{{ comment.body }}</p>
        <small class="text-muted">By: {{ comment.email }}</small>
      </li>
    </ul>
  </div>
</template>
