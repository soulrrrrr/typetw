<template>
  <div class="type-area-container">
    <!-- Displaying Content Text -->
    <div class="content-text">
      {{ targetText }}
    </div>

    <!-- Displaying User's Input -->
    <div class="type-area">
      <span v-for="(char, index) in displayedChars" :key="index" :class="char.class">{{ char.letter }}</span>
    </div>


  </div>
</template>


<style scoped>
.type-area {
  background-color: rgb(50, 52, 55);
  /* 背景顏色 */
  padding: 10px;
  font-size: 16px;
  display: inline-block;
  border: none;
  outline: none;
  font-weight: bold;
}

.content-text {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
}

.correct {
  color: white;
  /* 打對的字顏色 */
}

.incorrect {
  color: red;
  /* 打錯的字顏色 */
}

.placeholder {
  color: #737373;
  /* 淺灰色的未輸入的目標文字顏色 */
}

.cursor {
  color: yellow;
  /* cursor 顏色 */
  animation: blink 1s infinite;
}

@keyframes blink {
  50% {
    opacity: 0;
  }
}
</style>

<script>
// Import the poems data
import poemsData from '@/assets/poems_5.json';

export default {
  data() {
    // Randomly select a poem from the list
    const poem = poemsData.poems[Math.floor(Math.random() * poemsData.count)];
    return {
      targetText: poem.content,
      zhuyinText: poem.zhuyin,
      keyboardText: poem.keyboard,
      displayedText: [], // Initialize as empty, but you can use zhuyinText if you want it pre-filled
      currentKeyboardIndex: 0 // To track the current position in the keyboard representation
    };
  },
  computed: {
    displayedChars() {
      const chars = [];
      this.zhuyinText.split('').forEach((letter, index) => {
        const charClass = index < this.displayedText.length
          ? (this.displayedText[index].correct ? 'correct' : 'incorrect')
          : 'placeholder';
        chars.push({ letter: letter, class: charClass });
      });
      return chars;
    }
  },
  methods: {
    handleKeydown(event) {
      if (event.key === ' ') {
        event.preventDefault();
      }
      if (event.key === 'Backspace') {
        if (this.displayedText.length > 0) {
          // Remove the last character from displayedText
          this.displayedText.pop();
          this.currentKeyboardIndex -= 1;
        }
      } else {
        // Check if the pressed key matches the current position in the keyboard representation
        const isCorrect = this.keyboardText[this.currentKeyboardIndex] === event.key;
        if (isCorrect) {
          this.displayedText.push({ letter: this.zhuyinText[this.displayedText.length], correct: true });
        } else {
          this.displayedText.push({ letter: this.zhuyinText[this.displayedText.length], correct: false });
          // Decide if you want to increment currentKeyboardIndex on incorrect input or not
          // For now, we're not incrementing it.
        }
        this.currentKeyboardIndex++;
      }
    }

  },
  mounted() {
    window.addEventListener('keydown', this.handleKeydown);
  },
  beforeUnmount() {
    window.removeEventListener('keydown', this.handleKeydown);
  }
}
</script>
