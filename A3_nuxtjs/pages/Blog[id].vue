<!-- Amadeus Fidos u22526162-->

<template>
  <div v-if="pending" class="loading">Loading...</div>
  <div v-else-if="error" class="error">Error loading post</div>
  <div v-else-if="post" class="post">
    <h1>{{ post.title }}</h1>
    <div v-html="post.content"></div>
    <p>Author: {{ post.author?.name || 'Unknown' }}</p>
    <NuxtLink to="/">Back to posts</NuxtLink>
  </div>
</template>

<script setup>
const route = useRoute()
const { data: post, pending, error } = await useAsyncData(
  `post-${route.params.id}`,
  () => $fetch(`http://localhost:1337/api/blogs/${route.params.id}`, {
    params: {
      'populate[author]': true
    }
  })
)
</script>

<style>
.loading, .error {
  padding: 2rem;
  text-align: center;
}
.post {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}
</style>