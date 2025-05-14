<template>
  <article class="blog-card">
    <NuxtLink :to="`/posts/${post.id}`" class="blog-card-link">
      <div class="blog-card-content">
        <div class="blog-card-meta">
          <span class="blog-card-date">
            {{ formatDate(post.dateCreated) }}
          </span>
          <span v-if="post.author" class="blog-card-author">
            by {{ post.author.name }}
          </span>
        </div>
        <h3 class="blog-card-title">{{ post.title }}</h3>
        <p class="blog-card-text">
          {{ post.content }}
        </p>
      </div>
    </NuxtLink>
  </article>
</template>

<script setup>
const props = defineProps({
  post: {
    type: Object,
    required: true
  }
});

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  });
};

</script>

<style scoped src="@/assets/css/card.css"></style>