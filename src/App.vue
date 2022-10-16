<template>
  <div id="app">
    <h1>Endless Wordle</h1>

    <div v-for="guess in guesses">
      <WordField :guess="guess" :hints="wordHints" />
    </div>

    <div v-if="state === 'invalid-guess'" class="invalid-box">
      Invalid guess, word is not in database.
    </div>

    <div v-if="state === 'win' || state === 'loss'" class="result-box">
      <span v-if="state === 'win'">
        Awesome! You found the word: {{ currentWord }}.
      </span>

      <span v-if="state === 'loss'">
        Sorry, you did not find the word: {{ currentWord }}.
      </span>

      <a href="" @click="playAgain($event)">Play again?</a>
    </div>

    <Keyboard :hints="wordHints" @guess="onKeyboardClick" />
  </div>
</template>

<script>
import { wordleWords } from './wordle-list';
import WordField from './components/WordField';
import Keyboard from './components/Keyboard';

const initialGuessFields = [
  { value: '', type: 'blank' },
  { value: '', type: 'blank' },
  { value: '', type: 'blank' },
  { value: '', type: 'blank' },
  { value: '', type: 'blank' },
],

const jsonDeepClone = (val) => JSON.parse(JSON.stringify(val))

export default {
  name: 'App',
  components: {
    WordField,
    Keyboard,
  },
  data() {
    return {
      currentWord: null,
      currentGuessIndex: 0,
      currentLetterIndex: 0,
      guesses: [
        jsonDeepClone(initialGuessFields),
        jsonDeepClone(initialGuessFields),
        jsonDeepClone(initialGuessFields),
        jsonDeepClone(initialGuessFields),
        jsonDeepClone(initialGuessFields),
        jsonDeepClone(initialGuessFields)
      ],
      wordHints: [

      ],
      state: 'playing',
    };
  },
  created() {
    this.pickWordleWord();
  },
  methods: {
    pickWordleWord() {
      // see https://stackoverflow.com/a/5915122/1326808
      this.currentWord =
        wordleWords[Math.floor(Math.random() * wordleWords.length)];
    },
    currentGuessToString() {
      return this.guesses[this.currentGuessIndex]
        .reduce((guessString, letter) => `${guessString}${letter.value}`, '')
    },
    analyzeWord() {
      const { currentWord } = this;
      const guessedWord = this.currentGuessToString();

      if (!wordleWords.includes(guessedWord)) {
          this.state = 'invalid-guess'
          return;
      }

      this.state = 'playing'

      // FIXME unique hints
      const hints = this.guesses[this.currentGuessIndex].map(
        (letter, index) => {
          let type = 'no-match';

          if (currentWord.charAt(index) === letter.value) {
            type = 'perfect-match'
          } else if (currentWord.includes(letter.value)) {
            type = 'match'
          }

          return {
            type,
            value: letter.value
          }
        }
      )

      this.guesses[this.currentGuessIndex] = hints

      const hasWon = hints.every(hint => hint.type === 'perfect-match')

      if (hasWon) this.state = 'win'

      if (this.currentGuessIndex === 5 && this.state !== 'win') {
        this.state = 'loss'
      }

      this.wordHints = [
        ...this.wordHints,
        ...hints
      ]

      return true
    },
    onKeyboardClick(value) {
      if (this.state === 'win' || this.state === 'loss') {
        return
      }

      if (value === 'delete') {
        if (this.currentLetterIndex === 0) {
          return
        }

        this.currentLetterIndex += -1;

        this.guesses[this.currentGuessIndex][this.currentLetterIndex] = {
          value: '', type: 'blank'
        };

        this.state = 'playing'

      } else if (value === 'enter') {

        if (this.currentLetterIndex < 5) {
          return;
        }

        const validWord = this.analyzeWord();


        if (validWord) {
          this.currentGuessIndex += 1;
          this.currentLetterIndex = 0;
        }

      } else if (value.length === 1) {
        if (this.currentLetterIndex === 5) {
          return;
        }

        this.guesses[this.currentGuessIndex][this.currentLetterIndex] = {
          type: 'letter',
          value,
        };

        this.currentLetterIndex += 1;
      }
    },
    playAgain (e) {
      e.preventDefault()
      window.location.reload()
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.result-box {
  margin-top: 10px;
}

.invalid-box {
  margin-top: 10px;
  color: red;
}
</style>
