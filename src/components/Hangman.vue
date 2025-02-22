<script setup>
import { ref } from "vue";
import Game from "./Game.vue";
import PlayButton from "./PlayButton.vue";

const isGameStarted = ref(false);
</script>

<template>
  <div class="page-wrapper">
    <h1 class="page-title">Anton's Hangman</h1>
    <PlayButton
      v-if="!isGameStarted"
      @click="isGameStarted = true"
      text="Play"
    />
    <Game v-if="isGameStarted" />
  </div>
</template>

<style src="./Hangman.css" scoped />
