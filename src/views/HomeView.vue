<script setup lang="ts">
import axios from 'axios'
import { ref, onMounted, computed } from 'vue'
import type { Post } from '@/types/post'

const perPage = ref<number>(10)
const page = ref<number>(1)
const posts = ref<Post[]>([])
const search = ref<string>('')

onMounted(async () => {
  const { data }: { data: Post[] } = await axios.get('https://jsonplaceholder.typicode.com/posts')

  posts.value = data
})

const goToNextPage = () => {
  if (page.value * perPage.value < posts.value.length) {
    page.value++
  }
}

const filteredPosts = computed(() => {
  if (!search.value) {
    return posts.value
  }

  return posts.value.filter((post) => post.title.toLowerCase().includes(search.value.toLowerCase()))
})

const visiblePosts: Post[] = computed(() =>
  filteredPosts.value.slice(0, page.value * perPage.value),
)
</script>

<template>
  <div class="mb-3">
    <h1 class="text-left">Posts</h1>
    <div class="input-group mb-3">
      <input type="text" class="form-control" placeholder="Search posts..." v-model="search" />
    </div>
  </div>
  <div class="row g-2">
    <div class="col-12 col-sm-6 col-lg-4" v-for="post in visiblePosts" :key="post.id">
      <div class="card mt-0 h-100 w-100">
        <div class="card-body">
          <h5 class="card-title">{{ post.title }}</h5>
          <p class="card-text">{{ post.body }}</p>
          <a :href="`/post/${post.id}`" class="btn btn-primary">See details</a>
        </div>
      </div>
    </div>
  </div>
  <div class="mt-3 text-center" v-if="visiblePosts.length < filteredPosts.length">
    <button type="button" class="btn btn-light" @click="goToNextPage">Show More</button>
  </div>
</template>
