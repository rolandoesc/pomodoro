<template>
  <div class="container-fluid" id="holder">
    <div class="row">
      <div class="col-lg-6 offset-lg-3">
        <h3>{{properTitle}}</h3>
      </div>
      <div class="col-lg-6 offset-lg-3">
        <i class="material-icons" @click="addMinute(currentTimer)">add</i>
        <i class="material-icons" @click="subMinute(currentTimer)">remove</i>
        <div class="d-flex justify-content-center">
          <div class="clock">
            <p class="align-middle" id="time-left">
              <span v-if="!is_on_break">
                <span v-show="minutes_placer < 10">0</span>
                {{minutes_placer}}:
                <span v-show="seconds_placer < 10">0</span>
                {{seconds_placer}}
              </span>
              <span v-else>
                <span v-show="break_minutes < 10">0</span>
                {{break_minutes}}:
                <span v-show="break_seconds < 10">0</span>
                {{break_seconds}}
              </span>
            </p>
          </div>
        </div>
        <i class="material-icons" @click="playOrPauseTime('pause')">pause</i>
        <i class="material-icons" @click="playOrPauseTime('play')">play_arrow</i>

        <i class="material-icons" @click="reset(currentTimer)">repeat</i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      seconds_placer: 0,
      minutes_placer: 25,
      paused: true,
      break_minutes: 5,
      break_seconds: 0,
      is_on_break: false
    };
  },
  methods: {
    playOrPauseTime(action = "play") {
      const timer = setInterval(() => this.runTimer(timer), 1000)
      if (!this.paused) clearInterval(timer)
      if (action === "play") {
        this.paused = false;
      } else this.paused = true;
    },
    runTimer(fn) {
      const minute_type = !this.is_on_break ? "minutes_placer" : "break_minutes"
      const second_type = !this.is_on_break ? "seconds_placer" : "break_seconds"
      if (!this.paused) {
        // no está pausado
        if (this[minute_type] === 0 && this[second_type] === 0) {
          this.reset(this.is_on_break ? "timer":"break")
          this.is_on_break = !this.is_on_break;
        }
        if (this[minute_type] >= 0) {
          // Hay al menos 1 minuto corriendo
          this[second_type]--; // Resta 1 segundo
          if (this[second_type] < 0) {
            // Si los segundos están en 0
            this[second_type] = 59; // Cambia a 59 segundos
            this[minute_type]--; // Resta un minuto
          }
        }
      } else {
        clearInterval(fn)
        this.reset(this.is_on_break ? "timer":"break")
      }
    },
    addMinute(type = "timer") {
      return type === "timer" ? this.minutes_placer++ : this.break_minutes++;
    },
    subMinute(type = "timer") {
      const minute_type = type === "timer" ? "minutes_placer" : "break_minutes"
      if (this[minute_type] > 0)
        if (this[minute_type] === 1) this[minute_type] = 1;
        else this[minute_type]--;
      return this[minute_type];
    },
    reset(type = "timer") {
      if (type === "timer") {
        this.seconds_placer = 0;
        this.minutes_placer = 25;
      } else {
        this.break_minutes = 5;
        this.break_seconds = 0;
      }
      this.paused = true;
    }
  },
  computed: {
    currentTimer() {
      return this.is_on_break ? 'break' : 'timer'
    },
    properTitle() {
      if (!this.paused)
        if (this.is_on_break)
          return 'Chill yourself for a little bit! 😌'
        else return 'Pomodoro in progress! ⏲'
      else
        if (this.is_on_break)
          return 'Break timer paused'
        else return 'Pomodoro timer paused'
    }
  }
};
</script>

<style scoped>
.clock {
  height: 150px;
  width: 150px;
  border: solid #fafafa 3pt;
  border-radius: 50%;
}
.clock > p {
  margin-top: 38%;
  font-size: 1.5rem;

}
i {
  cursor: pointer;
  font-size: 1.75rem;
}
#holder {
  padding-top: 15vh;;
  padding-bottom: auto;
}
</style>