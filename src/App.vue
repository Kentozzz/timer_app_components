<template>
  <div id="app">
    <div>
      <label for="timeInput">カウントダウン時間（分）:</label>
      <input id="timeInput" type="number" v-model.number="inputMinutes" step="1" />
    </div>
    <div class="timer">{{ formatTime }}</div>
    <div class="controls">
      <button @click="setTime" :disabled="timerId !== null">セット</button>
      <button @click="startTimer" :disabled="timerId !== null || !timeSet">スタート</button>
      <button @click="stopTimer" :disabled="timerId === null">ストップ</button>
      <button @click="resetTimer">リセット</button>
    </div>
  </div>
</template>


<script setup>
import { ref, computed } from 'vue';

const inputMinutes = ref(0);
const startingTime = ref(0);
const time = ref(0);
let timerId = ref(null);
const timeSet = ref(false);

function setTime() {
  startingTime.value = inputMinutes.value * 60; // 入力された時間（分）を秒数に変換
  time.value = startingTime.value; // 残り時間（秒数）をセット
  timeSet.value = true;
}

function startTimer() {
  timerId.value = setInterval(() => {
    if (time.value > 0) {
      time.value -= 1; // 1秒減らす
      // タイマーが0になった時の処理
      if (time.value === 0) {
        setTimeout(() => {
          alert("時間になりました!");
        }, 10); 
        resetTimer(); // タイマーをリセット
      }
    }
  }, 1000);
}

function stopTimer() {
  clearInterval(timerId.value);
  timerId.value = null;
}

function resetTimer() {
  clearInterval(timerId.value);
  timerId.value = null;
  time.value = startingTime.value;
}

const formatTime = computed(() => {
  const minutes = Math.floor(time.value / 60);
  const seconds = ((time.value / 60) % 1) * 60;
  return `${minutes}:${seconds.toFixed(0).padStart(2, '0')}`;
});

</script>
