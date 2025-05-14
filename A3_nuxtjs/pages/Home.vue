<!-- Amadeus Fidos u22526162-->
<template>
  <main class="homepage">
    <section class="title-section">
      <h1 class="page-title">My Blog Site</h1>
    </section>

    <section class="blog-section">
      <div class="container">
        <div class="blog-grid">
          <BlogCard 
            v-for="blog in blogs?.data" 
            :key="blog.id" 
            :post="blog"
          />
        </div>

        <div v-if="isLoading" class="loading-state">
          Loading posts...
        </div>

        <div v-if="!isLoading && (!blogs?.data || blogs.data.length === 0)" class="empty-state">
          No posts found
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
const isLoading = ref(true);
const { data: blogs } = await useAsyncData('blogs', async () => {
  try {
    return await $fetch('http://localhost:1337/api/blogs', {
      params: {
        'populate[author]': true,
        sort: 'dateCreated:desc'
      }
    });
  } catch (error) {
    console.error('Error fetching blogs:', error);
    return { data: [] };
  } finally {
    isLoading.value = false;
  }
});
</script>

<style scoped src="@/assets/css/home.css"></style>