<template>
  <main id="home">
    <div class="container">
      <!-- container-banner -->
      <div class="container-banner">
        <div class="list-antrian">
          <h1 class="title-list">Nomor Antrian</h1>
          <div class="number-antrian">
            <span>{{ queueNumbers.queue }}</span>
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
            loading="lazy"
          ></iframe>
        </div>
      </div>
      <!-- end container-banner -->
    </div>

    <div class="container">
      <div class="grid-container">
        <div class="card" v-for="(number, index) in queueNumbers.lokets" :key="index">
          <h1 class="title-list">Loket {{ index + 1 }}</h1>
          <div class="card-body">
            <div class="number-antrian">{{ number }}</div>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div class="queue-form">
        <button @click="registerQueue" class="queue-button">Ambil Nomor Antrian</button>
      </div>
    </div>
  </main>
</template>

<script>
import socket from '@/lib/socket'
import axios from 'axios'

export default {
  data() {
    return {
      queueNumbers: {
        queue: 0,
        lokets: [0, 0, 0]
      },
      youtubeUrl: 'https://www.youtube.com/embed/RZb2_hxl-Cs?si=UGyBKvdehMOEzNa5&amp;start=6',
      speechVoice: null
    }
  },
  mounted() {
    socket.on('queue_update', (data) => {
      this.queueNumbers[data.loket] = data.queueNumber
    })

    socket.on('loket_empty', (data) => {
      this.queueNumbers[data.loket] = null
    })

    window.speechSynthesis.onvoiceschanged = () => {
      this.getIndonesianFemaleVoice()
    }
  },
  methods: {
    callNextNumber() {
      socket.emit('call_next')
    },
    registerQueue() {
      const emptyLocketIndex = this.queueNumbers.lokets.findIndex((number) => number === 0)
      if (emptyLocketIndex !== -1) {
        const nextQueueNumber = this.queueNumbers.queue ? this.queueNumbers.queue + 1 : 1
        this.queueNumbers.queue = nextQueueNumber
        this.queueNumbers.lokets[emptyLocketIndex] = nextQueueNumber

        const queueDataArray = this.queueNumbers.lokets.map((number, index) => ({
          loket: index + 1,
          queue: number,
          status: number === 0 ? 'waiting' : 'serving'
        }))

        axios
          .post('http://localhost:3000/update-queue', {
            updatedQueueData: queueDataArray
          })
          .then((response) => {
            if (response.data.success) {
            } else {
              window.alert(response.data.message)
            }
          })
          .catch((error) => {
            console.error('Error registering queue:', error)
            window.alert('Terjadi kesalahan saat mengirim data antrian.')
          })
      } else {
        window.alert('Semua loket sedang sibuk.')
      }
    },

    speakText(text) {
      const speech = new SpeechSynthesisUtterance()
      speech.text = text
      if (this.speechVoice) {
        speech.voice = this.speechVoice
      }
      window.speechSynthesis.speak(speech)
    },
    getIndonesianFemaleVoice() {
      const voices = window.speechSynthesis.getVoices()
      const indonesianFemaleVoice = voices.find((voice) => {
        return voice.lang.includes('id') && voice.name.includes('Wanita')
      })
      if (indonesianFemaleVoice) {
        this.speechVoice = indonesianFemaleVoice
      } else {
        console.warn('Tidak dapat menemukan suara bahasa Indonesia perempuan.')
      }
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
