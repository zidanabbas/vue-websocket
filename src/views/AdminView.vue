<template>
  <div class="container">
    <h1 class="title">Admin View</h1>
    <h2>Daftar Antrian</h2>
    <div class="list-data">
      <div class="btn-group gap-2">
        <button type="button" class="btn btn-primary btn-sm" @click="filterQueueByCounter(null)">
          Semua Loket
        </button>
        <button
          type="button"
          class="btn btn-primary btn-sm"
          v-for="loket in lokets"
          :key="loket"
          @click="filterQueueByCounter(loket)"
        >
          Loket {{ loket }}
        </button>
      </div>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">No</th>
            <th scope="col">Nomor Antrian</th>
            <th scope="col">Loket</th>
            <th scope="col">Status</th>
            <th scope="col">Opsi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(queue, index) in queues" :key="queue.queue">
            <td>{{ index + 1 }}</td>
            <td>{{ queue.queue }}</td>
            <td>{{ queue.loket ? 'Loket ' + queue.loket : '-' }}</td>
            <td>{{ queue.status }}</td>
            <td class="d-flex gap-2">
              <button
                type="button"
                class="btn btn-primary"
                @click="changeQueueStatus(queue.queue, 'success')"
                :disabled="queue.status !== 'waiting'"
              >
                Success
              </button>
              <button
                type="button"
                class="btn btn-danger"
                @click="changeQueueStatus(queue.queue, 'rejected')"
                :disabled="queue.status !== 'waiting'"
              >
                Reject
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import io from 'socket.io-client'

export default {
  data() {
    return {
      queues: []
    }
  },
  mounted() {
    this.socket = io('http://localhost:3000')

    this.fetchQueues()

    socket.on('queue_update', ({ queueNumber, newStatus }) => {
      // Perbarui status antrian secara real-time
      const updatedQueueIndex = this.queues.findIndex((queue) => queue.queueNumber === queueNumber)
      if (updatedQueueIndex !== -1) {
        this.$set(this.queues, updatedQueueIndex, {
          ...this.queues[updatedQueueIndex],
          status: newStatus
        })
      }
    })
  },

  methods: {
    async fetchQueues() {
      try {
        const response = await axios.get('http://localhost:3000/queue')
        if (response.status === 200) {
          this.queues = response.data.map((queue) => ({
            nomorAntrian: queue.queueNumber,
            loket: queue.loket,
            status: queue.status
          }))
        } else {
          console.error('Gagal mengambil data antrian:', response.statusText)
        }
      } catch (error) {
        console.error('Terjadi kesalahan saat mengambil data antrian:', error)
      }
    },
    async changeQueueStatus(queueNumber, newStatus) {
      try {
        const response = await axios.post('http://localhost:3000/approve-queue', {
          queueNumber: queueNumber
        })
        if (response.data.success) {
          const queueIndex = this.queues.findIndex((queue) => queue.nomorAntrian === queueNumber)
          if (queueIndex !== -1) {
            this.$set(this.queues, queueIndex, {
              ...this.queues[queueIndex],
              loket: null
            })
          }
          console.log('Status antrian berhasil diubah')
        } else {
          console.error(
            'Gagal mengubah status antrian:',
            response.data.message || response.statusText
          )
        }
      } catch (error) {
        console.error('Terjadi kesalahan saat mengubah status antrian:', error)
      }
    }
  }
}
</script>

<style>
.title {
  display: flex;
  width: 100%;
  padding: 5px 20px;
  font-size: 20px;
  font-weight: bold;
  color: white;
  background-color: #6f42c1;
}

h2 {
  padding: 5px 20px;
  width: 200px;
  font-size: 20px;
  font-weight: bold;
  color: black;
}

.list-data {
  display: flex-wrap;
  justify-content: center;
  padding: 20px 40px;
  border: 1px solid black;
  justify-content: center;
  align-items: center;
}
</style>
