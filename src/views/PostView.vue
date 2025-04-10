<script setup lang="ts">
import axios from 'axios'
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import type { IPost, IPostComment } from '@/types/post'
import Post from '@/components/Post.vue'

const post = ref<IPost | null>(null)
const route = useRoute()

onMounted(async () => {
  const { data }: { data: IPost } = await axios.get(
    `https://jsonplaceholder.typicode.com/posts/${route.params.id}`,
  )
  post.value = data
})
</script>

<template>
  <Post :post="post" :is-post-page="true" v-if="post" />
</template>
