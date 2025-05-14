<template>
  <main class="homepage">
    <section class="hero-section">
      <h1 class="hero-title">Welcome to My Blog</h1>
      <p class="hero-subtitle">Discover the latest articles and insights</p>
    </section>

    <section class="blog-section">
      <div class="container">
        <div class="category-filter">
          <select v-model="selectedCategory" class="filter-select">
            <option value="">All Categories</option>
            <option 
              v-for="category in categories" 
              :key="category.id" 
              :value="category.attributes.name"
            >
              {{ category.attributes.name }}
            </option>
          </select>
        </div>

        <div class="blog-grid">
          <BlogCard 
            v-for="post in filteredPosts" 
            :key="post.id" 
            :post="post" 
          />
        </div>

        <div v-if="isLoading" class="loading-state">
          Loading posts...
        </div>

        <div v-if="!isLoading && filteredPosts.length === 0" class="empty-state">
          No posts found. Check back later!
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
const selectedCategory = ref('')
const isLoading = ref(true)

// Fetch posts and categories
const { data: posts } = await useFetch('http://localhost:1337/api/posts?populate=*')
const { data: categories } = await useFetch('http://localhost:1337/api/categories')

isLoading.value = false

// Filter posts by category
const filteredPosts = computed(() => {
  if (!selectedCategory.value) return posts.value.data
  return posts.value.data.filter(post => 
    post.attributes.category?.data?.attributes?.name === selectedCategory.value
  )
})
</script>

<style scoped src="@/assets/css/home.css"></style>