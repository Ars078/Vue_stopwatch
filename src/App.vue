<template>
  <div class="content">
    <div class="stopwatch" v-for="stopwatch of stopwatches" :key="stopwatch.id">
      <span :class="{active_span: !stopwatch.toogle}">
        {{ stopwatch.hour }}:{{ stopwatch.min }}:{{ stopwatch.sec }}
      </span>
      <hr :class="{active_hr: !stopwatch.toogle}">
      <div class="btn">
        <button id="go">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg" @click="start(stopwatch.id)" v-if="stopwatch.toogle">
          <path d="M0 20V0L17 10L0 20Z" fill="#9E9E9E"/>
          </svg>
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg" @click="pause(stopwatch.id)" v-else>
          <rect x="7" width="3" height="20" fill="#9E9E9E" :class="{active_svg: !stopwatch.toogle}"/>
          <rect width="3" height="20" fill="#9E9E9E" :class="{active_svg: !stopwatch.toogle}"/>
          </svg>
        </button>
        <button id="stop" @click="reset(stopwatch.id)">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect width="20" height="20" fill="#9E9E9E" :class="{active_svg: !stopwatch.toogle}"/>
          </svg>
        </button>
      </div>
    </div>
  <div class="stopwatch add_stopwatch" @click="add()">
    <button class="add"><img src="./assets/plus.svg" alt="добавить"></button>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      id: 0,
      addStopwatch: {},
      stopwatches: [{
        id: 0,
        sec: 0,
        min: 0,
        hour: 0,
        toogle: true,
        interval: null
      }]
    }
  },

methods: {

  start (id) {
    for (let stopwatch of this.stopwatches) {
      if (stopwatch.id === id) {
        stopwatch.toogle = false
        stopwatch.interval = setInterval (() => {
          stopwatch.sec++;
          if (stopwatch.sec > 59) {
            stopwatch.sec = 0;
            stopwatch.sec++;
            stopwatch.min++;
            if (stopwatch.min > 59) {
              stopwatch.min = 0;
              stopwatch.hour++;
            }
          }
        }, 1000);
      }
    }
  },

  pause (id) {
    for (let stopwatch of this.stopwatches) {
      if (stopwatch.id === id) {
        stopwatch.toogle = true;
        clearInterval (stopwatch.interval);
      }
    }
  },

  reset (id) {
    for (let stopwatch of this.stopwatches) {
      if (stopwatch.id === id) {
        stopwatch.toogle = true;
        stopwatch.sec = 0;
        stopwatch.min = 0;
        stopwatch.hour = 0;
        clearInterval (stopwatch.interval);
      }
    }
  },

  add () {
    this.id++;
    for (let stopwatch of this.stopwatches) {
      stopwatch.toogle = true;
      this.addStopwatch = {
        id: this.id,
        sec: stopwatch.sec,
        min: stopwatch.min,
        hour: stopwatch.hour,
        toogle: true,
        interval: clearInterval (stopwatch.interval)
      }
    }
    this.stopwatches.push(this.addStopwatch);
  }

}
}
</script>

<style>

* {
  margin: 0;
  padding: 0;
  border: 0;
}

body {
  background: rgba(53, 54, 56, 1);
}

#app {
  width: 1200px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0 auto;
}

.content {
  display: flex;
  gap: 50px;
  flex-wrap: wrap;
  padding: 71px 212px;
}

.stopwatch {
  display: inline-block;
  background: rgba(105, 105, 105, 1);
}

span {
  display: inline-block;
  font-size: 22px;
  color: rgba(158, 158, 158, 1);
  padding: 12px 69.7px;
}

hr {
  border: 1px solid rgba(158, 158, 158, 1);
  margin: 0;
}

.btn {
  display: flex;
  padding: 20px 68.5px;
  gap: 48px;
}

button {
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  outline: none;
  width: 20px;
  height: 20px;
}

.add_stopwatch {
  width: 225px;
  height: 120px;
  cursor: pointer;
}

.add {
  position: relative;
  top: 40%;
}

.active_span {
  color: white;
}

.active_hr {
  border: 1px solid white;
}

.active_svg{
  fill: white;
}

@media (max-width: 1024px) {
  #app {
    width: 1024px;
  }
  .content {
  padding: 71px 134px;
  }
}

@media (max-width: 768px) {
  #app {
    width: auto;
  }
  .content {
  display: flex;
  flex-direction: column;
  align-content: center;
  padding: 71px 46px;
  }
}

</style>
