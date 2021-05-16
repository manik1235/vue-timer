<template>
  <div class="timer">
    <label :class="{ blink: isBlinking }">{{ getTimeRemaining() }}</label><br/>
    <button @click="nextState">{{ getNextState }}</button>
  </div>
</template>

<script>
export default {
  name: 'TheTimer',
  data () {
    return {
      currentState: this.beginningState,
      secondsRemaining: null,
      states: {
        Start: {
          nextName: 'Stop',
          action: 'changeTime'
        },
        Stop: {
          nextName: 'Start',
          action: 'signalFinished'
        }
      },
      delta: 1,
      isBlinking: false
    }
  },
  props: {
    seconds: {
      type: Number,
      default: 6
    },
    beginningState: {
      type: String,
      default: 'Stop'
    }
  },
  methods: {
    state () {
      if (!this.currentState) {
        this.currentState = this.beginningState
      }
      return this.currentState
    },
    getTimeRemaining (format = true) {
      if (this.secondsRemaining == null) {
        this.resetTimer()
      }
      if (format) {
        var seconds = this.secondsRemaining
        var minutes = seconds / 60
        var hours = minutes / 60
        var minutePad = ''
        var secondsPad = ''

        hours = Math.trunc(hours)
        minutes = Math.trunc(minutes - hours * 60)
        seconds = seconds - minutes * 60 - hours * 3600
        if (minutes < 10) {
          minutePad = 0
        }
        if (seconds < 10) {
          secondsPad = 0
        }

        return `${hours}:${minutePad}${minutes}:${secondsPad}${seconds}`
      }
      return this.secondsRemaining
    },
    nextState () {
      this.currentState = this.states[this.currentState].nextName
      this[this.states[this.currentState].action]()
    },
    startClock () {
      setTimeout(this.changeTime, 1000)
    },
    changeTime () {
      if (this.currentState === 'Start') {
        if (this.secondsRemaining === 0) {
          this.resetTimer()
          this.startClock()
          return
        }
        this.secondsRemaining -= 1
        if (this.secondsRemaining <= 0) {
          this.nextState()
        } else {
          this.startClock()
        }
      }
    },
    signalFinished () {
      this.$emit('timer-finished')
      this.isBlinking = true
    },
    resetTimer () {
      this.isBlinking = false
      this.secondsRemaining = this.seconds
    }
  },
  computed: {
    getNextState () {
      return this.states[this.currentState].nextName
    }
  }
}
</script>

<style scoped lang="scss">
.blink {
  animation: blinker 1s linear infinite;
}

@keyframes blinker {
  50% { opacity: 0; }
}
</style>
