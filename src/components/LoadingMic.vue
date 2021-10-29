<template>
  <div class="recordingLoader">
    <div class="recordingLoader__microphone"></div>
    <div class="recordingLoader__progress-bar">
      <div>
        <div ref="Bar" class="bar"></div>
      </div>
    </div>
    <p>{{ loadingPercentage }}%</p>
    <p>Запись сообщения</p>
  </div>
</template>

<script>
export default {
  name: 'RecordingLoader',
  data: () => ({
    isLoading: false,
    loadingPercentage: 0
  }),
  methods: {
    loadProgressBar () {
      const bar = this.$refs.Bar
      const id = setInterval(() => {
        if (this.loadingPercentage >= 100) {
          clearInterval(id)
          this.isLoading = false
        } else {
          this.loadingPercentage++
          bar.style.width = this.loadingPercentage + '%'
        }
      }, 50)
    }
  },
  mounted () {
    this.loadProgressBar()
  },
  watch: {
    loadingPercentage: function (newPercentage) {
      if (newPercentage === 100) {
        setTimeout(() => {
          this.$emit('send', false)
        }, 1000)
      }
    }
  }
}
</script>

<style scoped>
.recordingLoader {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 48px 0;
  position: relative;
  height: 100vh;
}

.recordingLoader__microphone {
  width: 146.55px;
  height: 69px;
  background: url('../assets/microphone.svg') center/cover;
  margin-bottom: 24px;
}

.recordingLoader > p {
  font-weight: 300;
  font-size: 20px;
  line-height: 23px;
  color: rgba(255, 255, 255, 0.6);
  padding-bottom: 12px;
}

.recordingLoader > p:last-child {
  font-weight: 300;
  font-size: 16px;
  line-height: 14px;
  color: rgba(255, 255, 255, 0.6);
  width: 259px;
  text-align: center;
}

.recordingLoader__progress-bar {
  width: 259px;
  height: 4px;
  padding-bottom: 30px;
}

.recordingLoader__progress-bar > div {
  width: 100%;
  background-color: #fff;
  border-radius: 5px;
}

.recordingLoader__progress-bar > div > div {
  width: 1%;
  height: 4px;
  background-color: #f6c866;
  border-radius: 5px;
}

.bar {
  transition: 0.1s all ease;
}
</style>
