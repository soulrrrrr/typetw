<template>
  <div>
    <h3>Time: {{ secondsPassed }} seconds</h3>
    <h3 v-if="!isTyping && secondsPassed !== 0">恭喜完成！正確率: {{ accuracy }}%</h3>
  </div>
</template>

<script>
export default {
  props: {
    isTyping: Boolean,
    startTime: Date,
    endTime: Date,
    secondsPassed: Number,
    correctCount: Number,
    totalTyped: Number
  },
  data() {
    return {
      timer: null
    }
  },
  computed: {
    accuracy() {
      if(this.totalTyped === 0) return 100; // To avoid division by zero
      return ((this.correctCount / this.totalTyped) * 100).toFixed(2); // Accuracy in percentage
    }
  },
  watch: {
    isTyping(newValue) {
      if (newValue) { // if typing started
        this.startTimer();
      } else { // if typing stopped
        this.stopTimer();
      }
    }
  },
  methods: {
    startTimer() {
      this.timer = setInterval(() => {
        this.$emit('tick');
      }, 1000);
    },
    stopTimer() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;
      }
    }
  },
  mounted() {
    if (this.isTyping) { // in case it's already true when component is mounted
      this.startTimer();
    }
  },
  beforeUnmount() {
    this.stopTimer();
  }
}
</script>
