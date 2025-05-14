<template>
  <article class="blog-card">
    <NuxtLink :to="`/posts/${post.id}`" class="blog-card-link">
      <div class="blog-card-image-container">
        <img 
          v-if="post.attributes.featuredImage?.data?.attributes?.url"
          :src="post.attributes.featuredImage.data.attributes.url" 
          :alt="post.attributes.title"
          class="blog-card-image"
        >
        <div v-else class="blog-card-image-placeholder"></div>
      </div>

      <div class="blog-card-content">
        <div class="blog-card-meta">
          <span class="blog-card-category" v-if="post.attributes.category">
            {{ post.attributes.category }}
          </span>
          <span class="blog-card-date">
            {{ formatDate(post.attributes.publishedAt) }}
          </span>
        </div>
        <h3 class="blog-card-title">{{ post.attributes.title }}</h3>
        <p class="blog-card-excerpt">
          {{ truncateText(post.attributes.content, 120) }}
        </p>
        <div class="blog-card-author">
          <img 
            v-if="post.attributes.author?.data?.attributes?.avatar?.data?.attributes?.url"
            :src="post.attributes.author.data.attributes.avatar.data.attributes.url" 
            :alt="post.attributes.author.data.attributes.name"
            class="blog-card-avatar"
          >
          <span>{{ post.attributes.author?.data?.attributes?.name || 'Unknown' }}</span>
        </div>
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
})

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  });
}

const truncateText = (text, maxLength) => {
  if (!text) return ''
  const stripped = text.replace(/<[^>]*>?/gm, '');
  return stripped.length > maxLength 
    ? stripped.substring(0, maxLength) + '...' 
    : stripped;
}
</script>

<style scoped src="@/assets/css/card.css"></style>