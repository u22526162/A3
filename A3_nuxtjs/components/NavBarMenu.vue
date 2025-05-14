<template>
  <nav class="navbar">
    <div class="navbar-container">
      <NuxtLink to="/Home" class="navbar-logo">My Blog</NuxtLink>
      <div class="navbar-links">
        <NuxtLink to="/Home" class="navbar-link">Home</NuxtLink>
        <NuxtLink to="/search" class="navbar-link">Search</NuxtLink>
      </div>
    </div>
  </nav>
</template>

<script>
export default {

}
</script>

<style scoped src="@/assets/css/navbar.css"></style>