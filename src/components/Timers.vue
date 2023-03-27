<template>
  <div class="container">
    <div class="timer-container">
      <div class="timers" v-for="timer in timers" :key="timer.id">
        <div class="field-timer" :class="{ playing: timer.isPlaying }">
          <div class="time" :class="{ playing: timer.isPlaying }">
            {{ formatTime(timer) }}
          </div>
          <hr>
          <div class="action-btn">
            <img class="play-start" src="./assets/треугольник.png" alt="" v-if="!timer.isPlaying"   @click="playTimer(timer)">
            <img class="play-start" src="./assets/ПаузаЗапуск.png" alt="" v-else @click="stopTimer(timer)">
            <img class="reset" src="./assets/квадрат.png" alt="" v-if="!timer.isPlaying" @click="resetTimer(timer)">
            <img class="reset" src="./assets/квадратЗапуск.png" alt="" v-else @click="resetTimer(timer)">
          </div>
        </div>
      </div>
      <div class="field-btn-add field-timer">
        <img src="./assets/plus.png" alt="" @click="addTimer">
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "TimerComponent",

  data() {
    return {
      timers: [],
      timer: {
        id: new Date(),
        seconds: 0,
        minute: 0,
        hours: 0,
        isPlaying: false,
        intervalId: null,
      },
    }
  },

  methods: {
    addTimer() {
      this.timers.push({ ...this.timer, id: new Date() })
    },

    playTimer(timer) {
      if (timer.isPlaying) return

      timer.isPlaying = true
      timer.intervalId = setInterval(() => {
        if (timer.seconds === 59) {
          timer.seconds = 0
          if (timer.minute === 59) {
            timer.minute = 0
            timer.hours++
          } else {
            timer.minute++
          }
        } else {
          timer.seconds++
        }
      }, 1000)
    },

    stopTimer(timer) {
      clearInterval(timer.intervalId)
      timer.isPlaying = false
    },

    resetTimer(timer) {
      clearInterval(timer.intervalId)
      timer.seconds = 0
      timer.minute = 0
      timer.hours = 0
      timer.isPlaying = false
    },

    formatTime(timer) {
      if (timer.hours > 0) {
        const formattedHours = timer.hours.toString().padStart(2, "0");
        const formattedMinute = timer.minute.toString().padStart(2, "0");
        const formattedSeconds = timer.seconds.toString().padStart(2, "0");
        return `${formattedHours}:${formattedMinute}:${formattedSeconds}`;
      } else if (timer.minute > 0) {
        const formattedMinute = timer.minute.toString().padStart(2, "0");
        const formattedSeconds = timer.seconds.toString().padStart(2, "0");
        return `${formattedMinute}:${formattedSeconds}`;
      } else {
        const formattedSeconds = Math.max(timer.seconds, 0).toString().padStart(2, "0");
        return `${formattedSeconds}`;
      }
    },
  }
}
</script>
<style scoped>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  width: 1024px;
}
.timer-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  position: relative;
}
.field-btn-add {
  display: flex;
  align-items: center;
  justify-content: center;
}
.field-timer {
  margin: 25px 25px;
  width: 225px;
  height: 120px;
  background-color: #696969;
}
.action-btn {
  margin: 20px 0 20px 0;
  display: flex;
  align-items: center;
  justify-content: space-around;
}
hr {
  margin-top: 17px;
  border: 1px solid #9E9E9E;
}
.time {
  font-family: 'Gotham Pro';
  font-style: normal;
  font-weight: 400;
  font-size: 22px;
  line-height: 21px;
  text-align: center;
  padding-top: 22px;
  color: #9E9E9E;
}
.playing {
  color: #FFFFFF;
}
.playing hr {
  border-color: #FFFFFF;
}
.play-start {
  margin-left: 23px;
}
.reset {
  margin-right: 29px;
}

@media (min-width: 1024px) {
  .timers {
    width: 30%;
  }
}

@media (min-width: 768px) and (max-width: 1023px) {
  .timers {
    width: 45%;
  }
}

@media (max-width: 767px) {
  .timers {
    width: 100%;
  }
}
</style>
