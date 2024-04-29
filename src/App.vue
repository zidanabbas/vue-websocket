<script setup>
import Navbar from './components/fragments/Navbar.vue'
import AdminView from './views/AdminView.vue'
import HomeView from './views/HomeView.vue'
// import { RouterView } from 'vue-router'
</script>

<template>
  <div>
    <Navbar />
    <RouterView />
  </div>
</template>
