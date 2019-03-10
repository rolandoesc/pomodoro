<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-6 offset-lg-3">
        <i class="material-icons" @click="addMinute">add</i>
        <i class="material-icons" @click="subMinute">remove</i>
        <div class="d-flex justify-content-center">

        <div class="clock">
          <p class="align-middle">
            <span v-if="!(initialMinutes < 0)">
              <span v-show="initialMinutes < 10">0</span>{{initialMinutes}}:<span v-show="initialTime < 10">0</span>{{initialTime}}
              
            </span>
            <span v-else-if="initialMinutes < 0">
              <span v-show="breakMinutes < 10">0</span>{{breakMinutes}}:<span v-show="breakSeconds < 10">0</span>{{breakSeconds}}
            </span>
          </p>
        </div>
        </div>
        <i class="material-icons" @click="paused = true">pause</i>
        <i class="material-icons" @click="paused = false">play_arrow</i>
        
        <i class="material-icons" @click="reset">repeat</i>

      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data: function () {
      return {
        initialTime: 0,
        initialMinutes: 25,
        paused: true,
        breakMinutes: 5,
        breakSeconds: 0
      }
    },
    methods: {
      secondsFormatter: function() {
        if (!this.paused) {
          if (this.initialMinutes >= 0) {
            // this.paused = this.initialTime === 0 && this.initialMinutes === 0 ? true : false 
            this.initialTime--;
            if (this.initialTime < 0) {
              this.initialTime = 59;
              this.initialMinutes --;
            }
          }

        }
      },
      addMinute: function() {
        return this.initialMinutes++
      },
      subMinute: function() {
        return this.initialMinutes > 0 ? this.initialMinutes-- : 0;
      },
      reset: function() {
        this.initialTime = 0
        this.initialMinutes = 25
        this.paused = true
      }
    },
    created: function() {
      let vm = this
      setInterval(vm.secondsFormatter, 1000)
    }
  }
</script>

<style scoped>
  .clock {
   height: 150px;
   width: 150px;
   border: solid black 5pt;
   border-radius: 50%;
   
  }
  .clock > p {
    margin-top: 40%;
  }
  i {
    cursor: pointer;
  }
</style>