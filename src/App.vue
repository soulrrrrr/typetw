<template>
  <div id="app">
    <h1>注音打字練習</h1>
    <type-timer :is-typing="isTyping" :start-time="startTime" :end-time="endTime" :seconds-passed="secondsPassed"
      :correct-count="correctCount" :total-typed="totalTyped" @tick="secondsPassed += 1"></type-timer>
    <type-area @startedTyping="startTimer" @finishedTyping="stopTimer" @correctTyped="incrementCorrectCount"
      @incorrectTyped="incrementIncorrectCount" @poem-refreshed="resetTypingData"></type-area>
    <h4>- 請使用英文輸入法</h4>

  </div>
</template>


<script>
import TypeArea from "./components/TypeArea.vue";
import TypeTimer from "./components/TypeTimer.vue";

export default {
  name: "app",
  components: {
    TypeArea,
    TypeTimer,
  },
  methods: {
    startTimer() {
      this.startTime = new Date();
      this.isTyping = true;
    },
    stopTimer() {
      this.endTime = new Date();
      this.isTyping = false;
    },
    incrementCorrectCount() {
      this.correctCount += 1;
      this.totalTyped += 1;
    },
    incrementIncorrectCount() {
      this.totalTyped += 1;
    },
    resetTypingData() {
      this.isTyping = false;
      this.startTime = null;
      this.endTime = null;
      this.secondsPassed = 0;
      this.correctCount = 0;
      this.totalTyped = 0;
    }
  },
  data() {
    return {
      isTyping: false,
      startTime: null,
      endTime: null,
      secondsPassed: 0,
      correctCount: 0,
      totalTyped: 0
    }
  }

};

</script>

<style>
/* Global styles */
.btn {
  padding: 0.8rem 1rem 0.7rem;
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  text-transform: capitalize;
}

.btn__danger {
  color: #fff;
  background-color: #ca3c3c;
  border-color: #bd2130;
}

.btn__filter {
  border-color: lightgrey;
}

.btn__danger:focus {
  outline-color: #c82333;
}

.btn__primary {
  color: #fff;
  background-color: #000;
}

.btn-group {
  display: flex;
  justify-content: space-between;
}

.btn-group>* {
  flex: 1 1 auto;
}

.btn-group>*+* {
  margin-left: 0.8rem;
}

.label-wrapper {
  margin: 0;
  flex: 0 0 100%;
  text-align: center;
}

[class*="__lg"] {
  display: inline-block;
  width: 100%;
  font-size: 1.9rem;
}

[class*="__lg"]:not(:last-child) {
  margin-bottom: 1rem;
}

@media screen and (min-width: 620px) {
  [class*="__lg"] {
    font-size: 2.4rem;
  }
}

.visually-hidden {
  position: absolute;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  clip-path: rect(1px, 1px, 1px, 1px);
  white-space: nowrap;
}

[class*="stack"]>* {
  margin-top: 0;
  margin-bottom: 0;
}

.stack-small>*+* {
  margin-top: 1.25rem;
}

.stack-large>*+* {
  margin-top: 2.5rem;
}

@media screen and (min-width: 550px) {
  .stack-small>*+* {
    margin-top: 1.4rem;
  }

  .stack-large>*+* {
    margin-top: 2.8rem;
  }
}

/* End global styles */
#app {
  background: #fff;
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  padding-top: 0;
  position: relative;
  box-shadow:
    0 2px 4px 0 rgba(0, 0, 0, 0.2),
    0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
}

@media screen and (min-width: 550px) {
  #app {
    padding: 4rem;
  }
}

#app>* {
  max-width: 60rem;
  margin-left: auto;
  margin-right: auto;
}

#app>form {
  max-width: 100%;
}

#app h1 {
  display: block;
  min-width: 100%;
  width: 100%;
  text-align: center;
  margin: 0;
  margin-bottom: 1rem;
}
</style>

