<template>
  <div id="app">
    <h1>點一下按鈕，對小婕說一句情話 💖</h1>

    <!-- 插入圖片 -->
    <img src="/harry.jpg" alt="harry" class="portrait" />

    <!-- 顯示情話 -->
    <p v-if="message" class="message">{{ message }}</p>

    <!-- 按鈕 -->
    <button @click="generateMessage" class="love-button">
      ❤️ 對小婕說情話 ❤️
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
          '我每天醒來的第一件事，就是想小婕。',
          '小婕的笑容，比陽光還溫暖。',
          '我願意陪郁婕走過未來每一天。',
          '小婕就是我世界裡最閃亮的星星。',
          '我愛你~',
          '好想小婕QQ',
          '即使世界崩塌，我也會緊緊牽著小婕的手。',
          '我每天最期待的，就是聽見小婕的聲音。',
          '如果愛有形狀，那一定是郁婕的樣子。',
          '我不需要寫詩，因為小婕本身就是一首詩。',
          '小婕，你就是我的心肝寶貝！',
          '欸，今天也要記得想我喔～',
          '跟你在一起，連呼吸都是甜的。',
          '小婕，你的笑聲是我一天中最好的音樂。',
          '我就是想跟你說：我很喜歡你～',
          '有你在身邊，什麼都不怕。',
          '小婕，你是我的快樂來源。',
          '想抱抱小婕，暖暖你的心。',
          '你知道嗎？我每天都在偷偷愛你。',
          '郁婕，你是我的小幸運。'
        ]
    }
  },
  methods: {
    generateMessage() {
      const index = Math.floor(Math.random() * this.messages.length)
      this.message = this.messages[index]

      // 觸發愛心動畫
      const button = document.querySelector('.love-button')
      button.classList.remove('active')
      void button.offsetWidth
      button.classList.add('active')
    }
  }
}
</script>

<style>
h1 {
  font-size: 1.4em; /* 比預設小一點 */
  color: #cc0066;
  margin-bottom: 20px;
}
.message {
  font-size: 1.2em; /* 訊息字體大小 */
  margin: 30px;
  transition: all 0.3s ease-in-out;
}

.love-button {
  font-size: 1.1em; /* 按鈕字體大小 */
  background-color: #ff6699;
  color: white;
  padding: 12px 30px;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  position: relative;
  outline: none;
}
body {
  margin: 0;
  background-color: #ffe6f0; /* 淡粉紅背景 */
  font-family: 'Arial', sans-serif;
}

#app {
  text-align: center;
  margin-top: 60px;
  color: #cc0066;
}

.portrait {
  display: block;
  width: 150px;
  height: auto;
  border-radius: 15px;
  margin: 20px auto; /* 水平置中 */
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.message {
  font-size: 1.6em;
  margin: 30px;
  transition: all 0.3s ease-in-out;
}

/* 愛心動畫按鈕 */
.love-button {
  background-color: #ff6699;
  color: white;
  font-size: 1.2em;
  padding: 12px 30px;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  position: relative;
  outline: none;
}

.love-button:hover {
  background-color: #ff3366;
  box-shadow: 0 0 15px #ff99cc;
}

/* 點擊動畫效果 */
.love-button.active {
  animation: heartbeat 0.4s ease;
}

@keyframes heartbeat {
  0% { transform: scale(1); }
  30% { transform: scale(1.2); }
  50% { transform: scale(0.95); }
  70% { transform: scale(1.1); }
  100% { transform: scale(1); }
}
</style>
