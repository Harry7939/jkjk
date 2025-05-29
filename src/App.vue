<template>
  <div id="app">
    <h1>點一下按鈕，看看會出現什麼話</h1>
    
    <!-- 顯示隨機訊息 -->
    <p v-if="message" style="font-size: 1.5em; margin: 20px;">{{ message }}</p>
    
    <!-- 按鈕 -->
    <button @click="generateMessage" style="padding: 10px 20px; font-size: 1em;">
      產生一句話
    </button>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      message: '',
      messages: [
        '今天也是元氣滿滿的一天！',
        '寫 Code 是一種藝術。',
        '你很棒，繼續加油！',
        'Vue 很讚，對吧？',
        '世界和平從微笑開始 😄',
        '錯誤是學習的一部分。',
        '別忘了喝水喔！'
      ]
    }
  },
  methods: {
    generateMessage() {
      const index = Math.floor(Math.random() * this.messages.length)
      this.message = this.messages[index]
    }
  }
}
</script>

<style>
#app {
  text-align: center;
  margin-top: 100px;
  font-family: 'Arial', sans-serif;
}
</style>
