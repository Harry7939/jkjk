<template>
  <div id="app">
    <p>
      今天新竹天氣：
      <img
        :src="`https://openweathermap.org/img/wn/${weatherDescription_a}@2x.png`"
        alt="新竹天氣圖示"
        style="width: 30px; height: 30px; vertical-align: middle;"
      />
      {{ temperature_a }}°C
    </p>

    <p>
      今天台南天氣：
      <img
        :src="`https://openweathermap.org/img/wn/${weatherDescription_b}@2x.png`"
        alt="台南天氣圖示"
        style="width: 30px; height: 30px; vertical-align: middle;"
      />
      {{ temperature_b }}°C
    </p>

    <p v-if="needUmbrella" class="glow-text">
      小婕今天要帶傘喔 🌂
    </p>

    <h1>給小婕的情話產生器 💖</h1>

    <!-- 動態圖片綁定 -->
    <img :src="currentImage" alt="harry" class="portrait" loading="lazy"/>

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
      temperature_a: null,
      temperature_b: null, 
      weatherDescription_a: '',
      weatherDescription_b: '',
      todayForecast: [],
      needUmbrella: false,
      currentImage: 'harry1.jpg',  // 預設圖片
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
      // 隨機選情話
      const index = Math.floor(Math.random() * this.messages.length)
      this.message = this.messages[index]

      // 隨機選圖片檔名 harry1.jpg ~ harry7.jpg
      //const imgIndex = Math.floor(Math.random() * 7) + 1
      //const fileName =  `harry${imgIndex}.jpg`
      //this.currentImage = `${fileName}`

      // 觸發按鈕動畫
      const button = document.querySelector('.love-button')
      button.classList.remove('active')
      void button.offsetWidth
      button.classList.add('active')
    }

  },
  mounted() {
  const apiKey = '87c59465145d28843021e531e8ccbbac'

  // 新竹天氣資料
  fetch(`https://api.openweathermap.org/data/2.5/weather?q=Hsinchu,TW&units=metric&appid=${apiKey}`)
    .then(res => res.json())
    .then(data => {
      this.temperature_a = Math.round(data.main.temp)
      this.weatherDescription_a = data.weather[0].icon
    })
    .catch(err => console.error('新竹天氣錯誤:', err))

  // 台南天氣資料
  fetch(`https://api.openweathermap.org/data/2.5/weather?q=Tainan,TW&units=metric&appid=${apiKey}`)
    .then(res => res.json())
    .then(data => {
      this.temperature_b = Math.round(data.main.temp)
      this.weatherDescription_b = data.weather[0].icon
    })
    .catch(err => console.error('台南天氣錯誤:', err))

  fetch(`https://api.openweathermap.org/data/2.5/forecast?q=Zhubei,TW&units=metric&appid=${apiKey}`)
    .then(res => res.json())
    .then(data => {
      const today = new Date().toISOString().split('T')[0]
      // 早上、中午、下午時間點
      const timesOfDay = [
        '06:00:00', '09:00:00',  // 早上
        '12:00:00', '15:00:00',  // 中午
        '18:00:00', '21:00:00'   // 下午
      ]
       this.todayForecast = data.list.filter(item =>
        timesOfDay.includes(item.dt_txt.slice(11)) &&
        item.dt_txt.includes(today)
      )

      // 判斷是否有雨
      this.needUmbrella = this.todayForecast.some(item =>
        item.weather.some(w => w.main.toLowerCase().includes('rain') || w.description.toLowerCase().includes('rain'))
      )
    })
    .catch(err => console.error('預報讀取錯誤:', err))
}

}
</script>


<style>
h1 {
  font-size: 2em; /* 比預設小一點 */
  color: #0f0e0e;
  margin-bottom: 20px;
}
.message {
  font-size: 2em;
  font-weight: bold;
  color: #1a1a1a;
  text-align: center;
  animation: rainbowGlow 5s infinite, bubbleMove 0.5s infinite ease-in-out;
  background: linear-gradient(45deg, #111111);
  background-size: 400% 400%;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

@keyframes bubbleMove {
  0%, 100% {
    transform: scale(1) translateY(0px);
  }
  50% {
    transform: scale(1.01) translateY(-2px);
  }
}

.love-button {
  font-size: 1.1em; /* 按鈕字體大小 */
  background-color: #e7e7e7;
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
  font-family: 'Arial', sans-serif;
  background: linear-gradient(13deg, #fbfafb, #eab5d6, #9481e9);
  background-size: 400% 400%;
  animation: dreamyBackground 5s ease infinite;
  overflow-x: hidden;
  touch-action: pan-y; /* 只允許垂直滑動 */
}

/* 漸層移動動畫 */
@keyframes dreamyBackground {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

#app {
  text-align: center;
  margin-top: 60px;
  color: #000000;
}

.portrait {
  display: block;
  width: 150px;
  height: auto;
  border-radius: 15px;
  margin: 20px auto; /* 水平置中 */
  box-shadow: 0 4px 12px rgba(3, 1, 1, 0.2);
}

.message {
  font-size: 1.6em;
  margin: 30px;
  transition: all 0.3s ease-in-out;
}

/* 愛心動畫按鈕 */
.love-button {
  background-color: #ffacd1;
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
  background-color: #ac6d97;
  box-shadow: 0 0 15px #0c0408;
}

/* 點擊動畫效果 */
.love-button.active {
  animation: heartbeat 0.4s ease;
}
.glow-text {
  color: black;
  font-weight: bold;
  animation: glow 1s infinite alternate;
}

@keyframes glow {
  0% {
    text-shadow: 0 0 5px #f4eaef, 0 0 10px #e6e1e3;
  }
  100% {
    text-shadow: 0 0 20px #d40aef, 0 0 30px #f308a8;
  }
}
@keyframes heartbeat {
  0% { transform: scale(1); }
  30% { transform: scale(1.2); }
  50% { transform: scale(0.95); }
  70% { transform: scale(1.1); }
  100% { transform: scale(1); }
}
</style>
