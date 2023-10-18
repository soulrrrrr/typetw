<template>
  <div>
    <!-- You can format the times more nicely if you prefer -->
    <!-- <p v-if="isTyping">Typing started at: {{ startTime }}</p>
    <p v-if="endTime">Typing finished at: {{ endTime }}</p> -->
    
    <p>Correct characters typed: {{ correctCount }}</p>
    <p>Total characters typed: {{ totalTyped }}</p>
    
    <p v-if="endTime">Time taken: {{ timeTaken }} seconds</p>
    <p>Accuracy: {{ accuracy }}%</p>
  </div>
</template>

<script>
export default {
  props: {
    isTyping: Boolean,
    startTime: Date,
    endTime: Date,
    correctCount: Number,
    totalTyped: Number
  },
  computed: {
    timeTaken() {
      if(this.startTime && this.endTime) {
        return ((this.endTime - this.startTime) / 1000).toFixed(2); // Time taken in seconds
      }
      return 0;
    },
    accuracy() {
      if(this.totalTyped === 0) return 100; // To avoid division by zero
      return ((this.correctCount / this.totalTyped) * 100).toFixed(2); // Accuracy in percentage
    }
  }
}
</script>
