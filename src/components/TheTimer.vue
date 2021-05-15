<template>
  <div class="timer">
    <label>{{ getTimeRemaining() }}</label><br/>
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
          action: 'stopTime'
        }
      },
      delta: 1
    }
  },
  props: {
    seconds: {
      type: Number,
      default: 60
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
      if (!this.secondsRemaining) {
        this.secondsRemaining = this.seconds
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
    changeTime () {
      if (this.currentState === 'Start') {
        this.secondsRemaining -= 1
        setTimeout(this.changeTime, 1000)
      }
    },
    stopTime () {
      console.log('stopTime')
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
</style>
