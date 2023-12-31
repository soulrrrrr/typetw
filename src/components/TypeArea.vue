<template>
  <div class="type-area-container">

    <!-- Displaying Title and Author -->
    <div class="title-author">
      <h2>{{ titleText }}</h2>
      <h2>{{ authorText }}</h2>
    </div>
    <!-- Displaying Content Text -->
    <div class="content-text">
      {{ targetText }}
    </div>

    <!-- Displaying User's Input -->
    <div class="type-area">
      <span v-for="(char, index) in displayedChars" :key="index" :class="char.class">{{ char.letter }}</span>
    </div>

    <button class="refresh-button" @click="refreshPoem">下一題</button>


  </div>
</template>


<style scoped>
.title-author {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0;
}

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

/* Style for the refresh button */
.refresh-button {
  padding: 10px 20px;
  /* Adjust padding to your preference */
  background-color: #DD8C6D;
  /* Background color */
  color: #fff;
  /* Text color */
  border: none;
  /* Remove button border */
  border-radius: 5px;
  /* Add some rounded corners */
  cursor: pointer;
  /* Show a pointer cursor on hover */
  font-size: 16px;
  /* Font size */
  font-weight: bold;
  /* Make the text bold */
  transition: background-color 0.2s ease-in-out;
  /* Smooth transition for background color */
  display: block;
  /* Ensures the button is treated as a block-level element */
  margin: 20px auto 0 auto;
  /* Top and bottom are 20px and 0, respectively; left and right are auto */


  /* Add a hover effect */
  &:hover {
    background-color: #BC6A51;
    /* Change background color on hover */
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
      titleText: poem.title,
      authorText: poem.author,
      targetText: poem.content,
      zhuyinText: poem.zhuyin,
      keyboardText: poem.keyboard,
      displayedText: [], // Initialize as empty, but you can use zhuyinText if you want it pre-filled
      currentKeyboardIndex: 0, // To track the current position in the keyboard representation
      zhuyin_to_keyboard: {
        'ㄅ': '1',
        'ㄆ': 'q',
        'ㄇ': 'a',
        'ㄈ': 'z',
        'ㄉ': '2',
        'ㄊ': 'w',
        'ㄋ': 's',
        'ㄌ': 'x',
        'ㄍ': 'e',
        'ㄎ': 'd',
        'ㄏ': 'c',
        'ㄐ': 'r',
        'ㄑ': 'f',
        'ㄒ': 'v',
        'ㄓ': '5',
        'ㄔ': 't',
        'ㄕ': 'g',
        'ㄖ': 'b',
        'ㄗ': 'y',
        'ㄘ': 'h',
        'ㄙ': 'n',
        'ㄧ': 'u',
        'ㄨ': 'j',
        'ㄩ': 'm',
        'ㄚ': '8',
        'ㄛ': 'i',
        'ㄜ': 'k',
        'ㄝ': ',',
        'ㄞ': '9',
        'ㄟ': 'o',
        'ㄠ': 'l',
        'ㄡ': '.',
        'ㄢ': '0',
        'ㄣ': 'p',
        'ㄤ': ';',
        'ㄥ': '/',
        'ㄦ': '-',
        '˙': '7',
        'ˊ': '6',
        'ˇ': '3',
        'ˋ': '4',
        '-': ' ',
      },
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
    refreshPoem() {
      // Randomly select a new poem from the list
      const poem = poemsData.poems[Math.floor(Math.random() * poemsData.count)];
      this.titleText = poem.title;
      this.authorText = poem.author;
      this.targetText = poem.content;
      this.zhuyinText = poem.zhuyin;
      this.keyboardText = poem.keyboard;
      this.displayedText = [];
      this.currentKeyboardIndex = 0;
      this.$emit('poem-refreshed');
    },
    handleKeydown(event) {
      event.preventDefault();
      console.log(event.key);

      if (this.displayedText.length === this.zhuyinText.length) {
        return;
      }

      if (this.displayedText.length === 0 && event.key.length === 1) {
        this.$emit('startedTyping'); // Notify the parent when the user starts typing the very first character.
      }

      if (event.key === 'Backspace') {
        if (this.displayedText.length > 0) {
          // Remove the last character from displayedText
          this.displayedText.pop();
          this.currentKeyboardIndex -= 1;
        }
      } else if (event.key.length === 1) {
        // translate zhuyin to keyboard
        const keyboardKey = this.zhuyin_to_keyboard[event.key];


        // Check if the pressed key matches the current position in the keyboard representation
        const correctKey = this.keyboardText[this.currentKeyboardIndex];
        const isCorrect = correctKey === event.key || correctKey === keyboardKey;
        if (isCorrect) {
          this.displayedText.push({ letter: this.zhuyinText[this.displayedText.length], correct: true });
          this.$emit('correctTyped'); // Notify the parent of a correct character typed.
        } else {
          this.displayedText.push({ letter: this.zhuyinText[this.displayedText.length], correct: false });
          this.$emit('incorrectTyped'); // Notify the parent of an incorrect character typed.
        }

        this.currentKeyboardIndex++;
      }

      if (this.displayedText.length === this.zhuyinText.length) {
        this.$emit('finishedTyping'); // Notify the parent when the user finishes typing.
        return;
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
