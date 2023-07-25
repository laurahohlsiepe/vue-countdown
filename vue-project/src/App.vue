<template>
<body class="bg-pink-300 font-mono">
  <header>
    <div>
       <h1 class="text-3xl font-bold text-center pt-12">
          Study Countdown for 15 or 30 min Intervalls 💗
       </h1>
    </div>
  </header>

  <div class="flex flex-col justify-center align-center m-12">
    <div class="m-auto">
      <select name="interval" id="interval" class="w-72 my-4" v-model="selectedValue">
        <option value="fifteen" class="text-center">15 min</option>
        <option value="thirty" class="text-center">30 min</option>
    </select>
    </div>

    <div class="m-auto">
      <button class="rounded-lg bg-pink-700 text-white text-center text-sm py-2.5 px-4 w-56" @click="chooseSection()">Choose your Countdown</button>
    </div>
  </div>

  <section class="mx-14 mt-8 py-12" v-if="fifteen">
    <div class="text-center text-3xl">
      <span>{{ twoDigitFormat(hour) }}</span>:<span>{{ twoDigitFormat(minutes) }}</span>:<span>{{ twoDigitFormat(seconds) }}</span>
    </div>

    <div class="flex justify-center gap-x-16 mt-10">
      <div>
      <button class="rounded-full bg-pink-700 text-white text-center w-20 text-sm py-2.5 mr-2 mb-2" @click="start()">Start</button>
    </div>
    <div>
      <button class="rounded-full bg-pink-700 text-white text-center w-20 text-sm py-2.5 mr-2 mb-2" @click="pause()">Pause</button>
    </div>
    <div>
      <button class="rounded-full bg-pink-700 text-white text-center w-20 text-sm py-2.5 mr-2 mb-2" @click="stop()">Stop</button>
    </div>
  </div>
  </section>

  <section class="mx-14 mt-8 py-12" v-if="thirty">
    <div class="text-center text-3xl">
      <span>{{ twoDigitFormat(hour) }}</span>:<span>{{ twoDigitFormat(minutes) }}</span>:<span>{{ twoDigitFormat(seconds) }}</span>
    </div>

    <div class="flex justify-center gap-x-16 mt-10">
      <div>
      <button class="rounded-full bg-pink-700 text-white w-20 text-sm py-2.5 mr-2 mb-2" @click="start()">Start</button>
    </div>
    <div>
      <button class="rounded-full bg-pink-700 text-white w-20 text-sm py-2.5 mr-2 mb-2" @click="pause()">Pause</button>
    </div>
    <div>
      <button class="rounded-full bg-pink-700 text-white w-20 text-sm py-2.5 mr-2 mb-2" @click="stop()">Stop</button>
    </div>
  </div>
  </section>

</body>
</template>

<script>
import { stringify } from 'postcss';

export default {
  name: 'App',
  data() {
    return {
      fifteen: false,
      thirty: false,
      selectedValue: '',
      hour: 0,
      minutes: 30,
      seconds: 0,
      timerInterval: null,
    }
  },

  methods: {
    chooseSection() {
      this.fifteen = this.selectedValue === 'fifteen';
      this.thirty = this.selectedValue === 'thirty';

      if (this.selectedValue === 'fifteen') {
        this.hour = 0;
        this.minutes = 15;
        this.seconds = 0;
        console.log('15')
      } else if (this.selectedValue === 'thirty') {
        this.hour = 0;
        this.minutes = 30;
        this.seconds = 0;        
        console.log('30')
      }
    },
    twoDigitFormat(value) {
      return value.toString().padStart(2, '0');
    },
    timer() {
      if (this.seconds > 0) {
        this.seconds--;
      } else {
        if (this.minutes > 0) {
          this.seconds = 59;
          this.minutes--;
        } else {
          if (this.hour > 0) {
            this.minutes = 59;
            this.seconds = 59;
            this.hour--;
          } else {
            this.stop();
          }
        }
      }
    },
    start() {
      if (this.timerInterval === null) {
        this.timerInterval = setInterval(this.timer, 1000);
      }
    },
    pause() {
      clearInterval(this.timerInterval);
      this.timerInterval = null;
    },

    stop() {
      clearInterval(this.timerInterval);
      this.timerInterval = null;
      if (this.selectedValue === 'fifteen') {
      this.hour = 0;
      this.minutes = 15;
      this.seconds = 0;
      } else if(this.selectedValue === 'thirty') {
      this.hour = 0;
      this.minutes = 30;
      this.seconds = 0;
      }
    },
  }
}
</script>

