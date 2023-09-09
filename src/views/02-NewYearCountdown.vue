<script setup>
import { ref } from "vue";

const year = ref('')
const days = ref('')
const hours = ref('')
const minutes = ref('')
const second = ref('')

const defYear = '0000'
const def = '00'

const currentYear = new Date().getFullYear()

const nextYear = new Date(`January 01 ${currentYear + 1} 00:00:00` )

const changeDays = () => {
  const currentTime = new Date();
  const timeLeft = nextYear - currentTime;
  const d = Math.floor(timeLeft / 1000 / 60 / 60 / 24)
  const h = Math.floor(timeLeft / 1000 / 60 / 60) % 24
  const m = Math.floor(timeLeft / 1000 / 60) % 60;
  const s = Math.floor(timeLeft / 1000) % 60;

  year.value = nextYear.getFullYear()
  days.value = d
  hours.value = h < 10 ? '0' + h : h
  minutes.value = m < 10 ? '0' + m : m
  second.value = s < 10 ? '0' + s : s
}

changeDays()
setInterval(changeDays,1000)
</script>

<template>
  <body>
  <div class="year">{{ year !== '' ? year : defYear }}</div>
  <h1>New Year Countdown</h1>
  <div class="count">
    <div class="time">
      <h2>{{ days !== '' ? days : def }}</h2>
      <small>days</small>
    </div>
    <div class="time">
      <h2>{{ hours !== '' ? hours : def }}</h2>
      <small>hours</small>
    </div>
    <div class="time">
      <h2>{{ minutes !== '' ? minutes : def }}</h2>
      <small>minutes</small>
    </div>
    <div class="time">
      <h2>{{ second !== '' ? second : def }}</h2>
      <small>seconds</small>
    </div>
  </div>
  </body>
</template>

<style scoped>

* {
  box-sizing: border-box;
}

body {
  background: url('./../assets/picture/fire.png') no-repeat center center;
  background-size: 100% 100%;
  height: 100vh;
  color: #fff;
  font-family: 'Lato', sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  margin: 0;
  overflow: hidden;
  /**
  加上下面这个 图片撑开整个页面
   */
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
}

body::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

body * {
  z-index: 1;
}

h1 {
  font-size: 60px;
  margin: -80px 0 40px;
}

.year {
  font-size: 200px;
  opacity: 0.2;
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
}

.count {
  display: flex;
  transform: scale(2);
}

.time {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 15px;
}

.time h2 {
  margin: 0 0 5px;
}

@media (max-width: 500px) {
  h1 {
    font-size: 45px;
  }

  .time {
    margin: 5px;
  }

  .time h2 {
    font-size: 12px;
    margin: 0;
  }

  .time small {
    font-size: 10px;
  }
}
</style>
