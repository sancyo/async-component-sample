<template>
  <div class="container">
    <h2 class="title">async component sample</h2>
    <div class="content">
      <div v-if="isLoading" class="loading"></div>
      <img v-if="!isLoading" class="post" :src="image" />
    </div>
    <div class="btn">
      <button @click="setContent()">呼ぶ</button>
      <button @click="deletePost()">削除</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      isLoading: false,
      image: '',
    }
  },
  created() {
    this.setContent()
  },
  methods: {
    async getPost() {
      const { data } = await axios.get(
        'https://designdock02.microcms.io/api/v1/posts/jpkq-bjw8',
        {
          headers: { 'X-API-KEY': process.env.API_KEY },
        }
      )
      return data.image.url
    },
    async setContent() {
      this.isLoading = true
      await new Promise((resolve) => setTimeout(resolve, 1000))
      this.image = await this.getPost()
      this.isLoading = false
    },
    deletePost() {
      this.image = ''
    },
  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  justify-content: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 30px;
  letter-spacing: 1px;
  margin: 30px 0;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.content {
  margin: 100px auto;
  background: #f3f3f4;
  width: 400px;
  height: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.loading {
  color: #323232;
  width: 30px;
  height: 30px;
  margin: 0 auto;
  border: 5px solid #323232;
  border-right: 5px solid transparent;
  border-radius: 50%;
  animation: rotate 1s linear infinite;
}

.post {
  width: 100%;
}

@keyframes rotate {
  100% {
    transform: rotate(360deg);
  }
}
</style>
