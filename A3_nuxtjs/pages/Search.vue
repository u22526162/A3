<!-- Amadeus Fidos u22526162-->

<template>
  <div class="search-page">
    <div class="search-container">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search posts or authors..."
        class="search-input"
        @input="performSearch"
      />
      <button class="search-button" @click="performSearch">
        <svg class="search-icon" viewBox="0 0 24 24">
          <path d="M15.5 14h-.79l-.28-.27a6.5 6.5 0 1 0-.7.7l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0A4.5 4.5 0 1 1 14 9.5 4.5 4.5 0 0 1 9.5 14z"/>
        </svg>
      </button>
    </div>

    <div v-if="searchResults.length > 0" class="results-container">
      <div v-for="result in searchResults" :key="result.id" class="result-card">
        <NuxtLink :to="`/posts/${result.id}`" class="result-link">
          <h3 class="result-title">{{ result.title }}</h3>
          <p class="result-author">By {{ result.author?.name || 'Unknown' }}</p>
          <p class="result-snippet">{{ truncateText(result.content, 120) }}</p>
        </NuxtLink>
      </div>
    </div>

    <div v-else-if="searchQuery && !isLoading" class="no-results">
      No results found for "{{ searchQuery }}"
    </div>

    <div v-if="isLoading" class="loading-indicator">
      Searching...
    </div>
  </div>
</template>

<script setup>
const searchQuery = ref('');
const searchResults = ref([]);
const isLoading = ref(false);
const debounceTimer = ref(null);

const performSearch = () => {
  clearTimeout(debounceTimer.value);
  
  if (!searchQuery.value.trim()) {
    searchResults.value = [];
    return;
  }

  isLoading.value = true;
  debounceTimer.value = setTimeout(async () => {
    try {
      const { data } = await $fetch('http://localhost:1337/api/blogs', {
        params: {
          'filters[$or][0][title][$containsi]': searchQuery.value,
          'filters[$or][1][author][name][$containsi]': searchQuery.value,
          'populate[author]': true
        }
      });
      searchResults.value = data;
    } catch (error) {
      console.error('Search failed:', error);
      searchResults.value = [];
    } finally {
      isLoading.value = false;
    }
  }, 300);
}

const truncateText = (text, maxLength) => {
  if (!text) return '';
  const stripped = text.replace(/<[^>]*>?/gm, '');
  return stripped.length > maxLength 
    ? stripped.substring(0, maxLength) + '...' 
    : stripped;
}
</script>

<style scoped src="@/assets/css/search.css"></style>