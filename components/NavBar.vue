<template>
  <nav class="navbar">
    <div class="navbar-container">
      <NuxtLink to="/" class="navbar-logo">My Blog</NuxtLink>
      <div class="navbar-links">
        <NuxtLink to="/" class="navbar-link">Home</NuxtLink>
        <NuxtLink to="/search" class="navbar-link">Search</NuxtLink>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  // No inline styles here
}
</script>

<style scoped src="@/assets/css/navbar.css"></style>