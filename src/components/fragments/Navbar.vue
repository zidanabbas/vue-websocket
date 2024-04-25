<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <header class="header">
    <div class="brand">
      <div class="logo">
        <img src="@/assets/djp-pajak.jpg" width="100px" alt="logo" />
      </div>
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
  background-color: #fcec27;
}

.brand {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-weight: bold;
  font-size: large;
  padding: 0 5px;
  gap: 10px;
}

.brand .logo {
  display: flex;
  width: 100px;
  justify-content: center;
  align-items: center;
}

.brand a {
  text-decoration: none;
  color: #343a40;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

.brand a:hover {
  color: rgb(72, 72, 72);
}

.time {
  display: flex;
  padding: 2px;
  color: #343a40;
  font-size: 1rem;
}
</style>
