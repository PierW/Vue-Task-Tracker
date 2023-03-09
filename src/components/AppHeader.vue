<script setup>
</script>

<template>
    <header>
        <nav class="container">
            <router-link to="/" class="branding" title="vai alla home">
                <img src="../assets/Vue_Logo_Black.png" alt="logo">
                <h2>Vue Task Tracker</h2>
            </router-link>
            <ul class="nav-routes">
                <li>
                    <RouterLink to="/">Home</RouterLink>
                </li>
                <li>
                    <router-link :to="{name : 'about'}">About</router-link>
                </li>
            </ul>
        </nav>
    </header>
</template>

<style lang="scss" scoped>
header {
  background-color: #f1f1f1;
  nav {
    display: flex;
    align-items: center;
    padding: 25px 16px;
    .branding {
      display: flex;
      align-items: center;
      gap: 8px;
      text-decoration: none;
      color: #000;
      img {
        max-width: 50px;
      }
      h1 {
        font-size: 24px;
      }
    }
    .nav-routes {
      display: flex;
      flex: 1;
      justify-content: flex-end;
      gap: 12px;
      list-style: none;
      a {
        text-decoration: none;
        color: inherit;
      }
    }
  }
}
</style>