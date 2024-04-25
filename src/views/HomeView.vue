<template>
  <main class="container">
    <div class="container-list-antrian">
      <div class="list-antrian">
        <h1 class="title-list">List Antrian</h1>
        <div class="number-antrian">
          <span>1</span>
        </div>
      </div>
    </div>
    <div class="frame">
      <iframe
        width="100%"
        height="100%"
        :src="youtubeUrl"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
        referrerpolicy="strict-origin-when-cross-origin"
        allowfullscreen
      ></iframe>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      youtubeUrl: 'https://www.youtube.com/embed/RZb2_hxl-Cs?si=UGyBKvdehMOEzNa5&amp;start=6'
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  width: 100%;
  padding: 5px 40px;
  background-color: #232946;
  gap: 20px;
}

.container-list-antrian {
  width: 40%;
  height: 400px;
  /* border: 1px solid black; */
}

.list-antrian {
  display: flex-wrap;
  justify-content: center;
  gap: 20px;
  width: 100%;
  height: 100%;
  background-color: #ffd803;
}

.list-antrian .title-list {
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  color: white;
}

.list-antrian .number-antrian {
  display: flex;
  margin-top: 70px;
  min-height: 100px;
  justify-content: center;
  align-items: center;
  width: 100%;
  /* border: 1px solid black; */
}

.list-antrian .number-antrian span {
  display: inline-block;
  height: 100%;
  font-size: 100px;
  font-weight: bold;
  color: white;
  /* border: 1px solid black; */
}

.frame {
  width: 60%;
  height: 400px;
  /* background-color: green; */
}
</style>
