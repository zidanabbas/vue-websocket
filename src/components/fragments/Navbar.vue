<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <header class="header">
    <div class="brand">
      <RouterLink to="/">BayarPajak</RouterLink>
    </div>
    <div class="time">
      <span>{{ currentTime }}</span>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      currentTime: new Date().toLocaleTimeString()
    }
  },
  mounted() {
    setInterval(() => {
      this.currentTime = new Date().toLocaleTimeString()
    }, 1000)
  }
}
</script>

<style scoped>
.header {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 60px;
  padding: 0 40px;
  background-color: #faae2b;
}

.brand {
  font-weight: bold;
  font-size: large;
  color: white;
  /* border: 1px solid white; */
  padding: 5px;
}

.brand a {
  text-decoration: none;
  color: white;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

.brand a:hover {
  color: black;
}

.time {
  display: flex;
  padding: 2px;
  color: whitesmoke;
  font-size: 1rem;
}
</style>
