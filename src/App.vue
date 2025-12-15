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
    <p v-else class="glow-text">
      小婕今天不用帶傘喔 ☀️
    </p>

    <h1>給小婕的情話產生器 💖</h1>
    <p>
    如果妳看到這段話，<br>
    代表我正在某個地方，靜靜地等妳。<br><br>

    不是要求妳回頭，<br>
    也不是逼妳做任何決定，<br>
    只是想讓妳知道——<br><br>

    有一個人，<br>
    把妳放在心裡很重要的位置，<br>
    也願意尊重妳現在的一切選擇。<br><br>

    如果哪一天，<br>
    妳想被好好聽、好好珍惜，<br>
    我一直都在。
    </p>

    <!-- 動態圖片綁定 -->
    <img :src="currentImage" alt="harry" class="portrait" loading="lazy"/>

    <!-- 顯示情話 -->
    <p v-if="currentMessage" class="message">
      {{ currentMessage }}
    </p>

    <!-- 按鈕 -->
    <button @click="switchType('love'); generateMessage()" class="love-button">
    ❤️ 對小婕說情話
    </button>
        <button @click="switchType('love'); generateMessage()" class="sorry-button" style="margin-left: 5px;">
       跟小婕道歉 😢
    </button>

    <footer>
    <button @click="showReleaseNote = !showReleaseNote" class="release-toggle-button">
      {{ showReleaseNote ? 'Hide Release Notes...' : 'View Release Notes...' }}
    </button>

    <div v-if="showReleaseNote" class="release-footer">
      <p><strong>Version：</strong>{{ version }}</p>
      <p><strong>Release Notes：</strong></p>
      <ul>
        <li v-for="(note, index) in releaseNotes" :key="index">{{ note }}</li>
      </ul>
    </div>
  </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      showReleaseNote: false,
      version: 'v1.1',
      releaseNotes: [
        '新增道歉模式 ❤️‍🩹',
        '按鈕樣式優化，支援漸層與圓角',
        '加入「是否要帶傘」判斷功能（下雨機率超過1/3）',
        '修復訊息重複出現的小問題',
      ],
      message: '',
      temperature_a: null,
      temperature_b: null, 
      weatherDescription_a: '',
      weatherDescription_b: '',
      todayForecast: [],
      needUmbrella: false,
      currentImage: 'harry1.jpg',  // 預設圖片
      currentType: 'love', // or 'sorry'
      messageList: {
        love: [
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
          '我就是想跟你說：我很喜歡你～'
        ],
        sorry: [
          '對不起小婕，我真的不是故意的…QQ',
          '如果一句對不起能讓你笑，我願意說一百次。',
          '小婕，你是我最在乎的人，我不想失去你。',
          '我知道我錯了，請再給我一次機會好嗎？',
          '小婕，我的世界少了你，就不完整了。',
          '對不起讓你難過，我心也跟著痛了。',
          '你的眼淚，是我最不想看到的東西。',
          '我不完美，但我願意為你改變。',
          '我沒有資格要求原諒，但我會努力補償。',
          '小婕，你生氣的樣子也好可愛，可是我更想看到你笑。',
          '對不起，我會記住今天的教訓，只為不再讓你傷心。',
          '我真的很笨，總是在你傷心後才知道自己多在乎你。',
          '請不要離開我，我已經離不開你了。',
          '我想你原諒我，不是因為我值得，而是因為我真的愛你。',
          '這次我真的學乖了，只希望還有機會讓你開心。'
        ]
      },
      currentMessage: ''
    }
  },
  methods: {
  generateMessage() {
    const messages = this.messageList[this.currentType];
    const index = Math.floor(Math.random() * messages.length);
    this.currentMessage = messages[index];
  },
  switchType(type) {
    this.currentType = type;
    this.currentMessage = ''; // 或保留目前內容
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
      const rainyCount = this.todayForecast.filter(item =>
      item.weather.some(w =>
        w.main.toLowerCase().includes('rain') ||
        w.description.toLowerCase().includes('rain')
      )
    ).length;

    this.needUmbrella = (rainyCount / this.todayForecast.length) > (1 / 3);
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

.love-button {
  background: linear-gradient(to right, #e7aabe, #ee90ca);
  color: white;
  font-size: 1.2em;
  padding: 12px 30px;
  border: none;
  border-top-left-radius: 50px;
  border-bottom-left-radius: 50px;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  position: relative;
  outline: none;
}

.sorry-button {
  background: linear-gradient(to right, #ee90ca, #e7aabe);
  color: white;
  font-size: 1.2em;
  padding: 12px 30px;
  border: none;
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
  border-top-right-radius: 50px;
  border-bottom-right-radius: 50px;
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

.sorry-button:hover {
  background-color: #ac6d97;
  box-shadow: 0 0 15px #0c0408;
}

/* 點擊動畫效果 */
.love-sorry.active {
  animation: heartbeat 0.4s ease;
}
.glow-text {
  color: black;
  font-weight: bold;
  animation: glow 1s infinite alternate;
}
.release-footer {
  margin-top: 40px;
  padding: 20px;
  background-color: #fef3f7;
  color: #333;
  border-top: 2px solid #ffacd1;
  font-size: 0.5em;
}
.release-footer ul {
  padding-left: 1sem;
  margin-top: 5px;
}
.release-toggle-button {
  background-color: #ffd7eb;
  border: none;
  border-radius: 5px;
  padding: 6px 14px;     /* 更小的上下左右間距 */
  font-size: 0.9em;       /* 字體縮小一點 */
  cursor: pointer;
  margin-top: 20px;       /* 上方間距也微調 */
}
@keyframes glow {
  0% {
    text-shadow: 0 0 5px #f4eaef, 0 0 10px #e6e1e3;
  }
  100% {
    text-shadow: 0 0 20px #de6eec, 0 0 30px #f13eb9;
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
