<script lang="ts" setup>
import { defineProps, withDefaults } from 'vue'
import type { IPost } from '@/types/post'
import PostComments from './PostComments.vue'

const props = withDefaults(
  defineProps<{
    post: IPost | null
    isPostPage?: boolean
  }>(),
  {
    post: null,
    isPostPage: false,
  },
)
</script>

<template>
  <div class="post" :class="{ 'mt-0 h-100 w-100': !props.isPostPage }">
    <div v-if="post" class="card" :class="{ 'mt-0 h-100 w-100': !props.isPostPage }">
      <div class="card-body flex-column d-flex justify-content-between">
        <div class="card-info mb-3">
          <h1 class="card-title">{{ post.title }}</h1>
          <p class="card-text">{{ post.body }}</p>
        </div>
        <a v-if="!props.isPostPage" :href="`/post/${post.id}`" class="btn btn-primary"
          >See details</a
        >
      </div>
    </div>

    <PostComments v-if="isPostPage" :post-id="post?.id" />
  </div>
</template>
