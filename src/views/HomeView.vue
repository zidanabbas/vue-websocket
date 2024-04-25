<template>
  <main id="home">
    <div class="container">
      <!-- container-banner -->
      <div class="container-banner">
        <div class="list-antrian">
          <h1 class="title-list">List Antrian</h1>
          <div class="number-antrian">
            <span>1</span>
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
      </div>
      <!-- end container-banner -->
    </div>
    <div class="container">
      <div class="grid-container">
        <div class="card">
          <h1 class="title-list">Loket 1</h1>
          <div class="card-body">
            <div class="number-antrian">20</div>
          </div>
        </div>
        <div class="card">
          <h1 class="title-list">Loket 2</h1>
          <div class="card-body">
            <div class="number-antrian">20</div>
          </div>
        </div>
        <div class="card">
          <h1 class="title-list">Loket 3</h1>
          <div class="card-body">
            <div class="number-antrian">20</div>
          </div>
        </div>
      </div>
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
  padding: 20px 40px;
  /* background-color: #6f42c1; */
  /* border: 1px solid white; */
}

.container-banner {
  display: flex;
  align-items: center;
  width: 100%;
  gap: 20px;
  padding: 0 20px;
  /* background-color: aqua; */
  /* border: 1px solid black; */
}

.list-antrian {
  display: flex-wrap;
  justify-content: center;
  width: 500px;
  height: 325px;
  background-color: #6f42c1;
  border-radius: 20px;
}

.title-list {
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
  height: 325px;
}

.card {
  justify-self: center;
  width: 350px;
  height: 200px;
  background-color: #6f42c1;
  color: white;
  padding: 10px;
  border-radius: 20px;
  overflow: hidden;
}

.card-body {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 80%;
  /* border: 1px solid black; */
}

.card-body .number-antrian {
  font-size: 50px;
  font-weight: bold;
}

.grid-container {
  display: grid;
  width: 100%;
  padding: 5px;
  grid-auto-flow: column;
  align-items: center;
  justify-content: center;
  gap: 20px;
}
</style>
