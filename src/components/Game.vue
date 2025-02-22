<script setup>
import { ref } from "vue";
import { generate } from "random-words";
import PlayButton from "./PlayButton.vue";

const word = ref(generate({ exactly: 1, wordsPerString: 1 })[0]);
const lettersAttempted = ref([]);
const isGameOverSuccess = ref(false);
const isGameOverFailure = ref(false);
const wrongGuesses = ref(0);

function attemptLetter(letter) {
  lettersAttempted.value.push(letter);

  if (!word.value.toUpperCase().includes(letter)) {
    wrongGuesses.value++;

    if (wrongGuesses.value === 6) {
      isGameOverFailure.value = true;
    }
  }

  let isDone = true;

  for (let i = 0; i < word.value.length; i++) {
    if (!lettersAttempted.value.includes(word.value.toUpperCase().charAt(i))) {
      console.log("im not doneee");
      isDone = false;
      break;
    }
  }

  if (isDone) {
    console.log("isDOne is trueee");
    isGameOverSuccess.value = true;
  }
}

function restart() {
  lettersAttempted.value = [];
  isGameOverFailure.value = false;
  isGameOverSuccess.value = false;
  wrongGuesses.value = 0;
  word.value = generate({ exactly: 1, wordsPerString: 1 })[0];
}

console.log(word.value);
</script>

<template>
  <div class="word-wrapper" v-if="typeof word === 'string'">
    <div v-for="i in word.length" :key="i">
      {{
        lettersAttempted.includes(word.toUpperCase().charAt(i - 1))
          ? word.toUpperCase().charAt(i - 1)
          : "_"
      }}
    </div>
  </div>
  <div class="letter-buttons-wrapper">
    <button
      v-for="n in 26"
      :key="String.fromCharCode('A'.charCodeAt(0) + n - 1)"
      @click="
        () => attemptLetter(String.fromCharCode('A'.charCodeAt(0) + n - 1))
      "
      class="letter-button"
      :class="{
        'letter-button-attempted': lettersAttempted.includes(
          String.fromCharCode('A'.charCodeAt(0) + n - 1)
        ),
      }"
      :disabled="
        isGameOverSuccess ||
        isGameOverFailure ||
        lettersAttempted.includes(
          String.fromCharCode('A'.charCodeAt(0) + n - 1)
        )
      "
    >
      {{ String.fromCharCode("A".charCodeAt(0) + n - 1) }}
    </button>
  </div>
  <div class="image-wrapper">
    <img :src="`/hangman/hangman-${wrongGuesses}.svg`" alt="Hangman Image" />
  </div>
  <div v-if="isGameOverFailure" class="game-over-modal">
    <div class="content">
      <img src="/hangman/lost.gif" alt="You lost image" />
      <div class="text">
        Game over! The correct word was: <b>{{ word }}.</b>
      </div>
      <PlayButton @click="restart" text="Play">Play again</PlayButton>
    </div>
  </div>
  <div v-if="isGameOverSuccess" class="victory-modal">
    <div class="content">
      <img src="/hangman/victory.gif" alt="You won image" />
      <div class="congratulations-title">Congratulations!</div>
      <div class="text">
        You found the word: <b>{{ word }}.</b>
      </div>
      <PlayButton @click="restart" text="Play">Play again</PlayButton>
    </div>
  </div>
</template>

<style src="./Game.css" scoped />
