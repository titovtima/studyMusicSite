<template>
  <div class="text-center"><div class="inline-block"><Piano :pressed="pressedKeys"/></div></div>
  <div class="text-center mt-5">
    <button @click="pickUpKey()">Новая тональность</button>
    <button @click="pickUpDegree(key)" :disabled="toValue(key) == null">Новая ступень</button>
    <button @click="playKeyChord(key)" :disabled="toValue(key) == null">Повторить тональность</button>
    <button @click="playDegree()" :disabled="toValue(degree) == null">Повторить ступень</button>
  </div>
  <div class="text-center">
    <button class="degree-button" @click="makeGuess(0)" :disabled="toValue(degree) == null">I</button>
    <button class="degree-button" @click="makeGuess(1)" :disabled="toValue(degree) == null">II</button>
    <button class="degree-button" @click="makeGuess(2)" :disabled="toValue(degree) == null">III</button>
    <button class="degree-button" @click="makeGuess(3)" :disabled="toValue(degree) == null">IV</button>
    <button class="degree-button" @click="makeGuess(4)" :disabled="toValue(degree) == null">V</button>
    <button class="degree-button" @click="makeGuess(5)" :disabled="toValue(degree) == null">VI</button>
    <button class="degree-button" @click="makeGuess(6)" :disabled="toValue(degree) == null">VII</button>
  </div>
  <div id="result" class="text-2xl"></div>
</template>

<script setup>
import {getCircleKeys, getDegree} from "@titovtima/music-theory";

useHead({
  title: "Угадай ступень тональности"
});

const pressedKeys = ref([]);
const key = ref(null);
const degree = ref(null);

function pickUpKey(mode) {
  let i;
  let keys = getCircleKeys();
  if (mode != null)
    i = Math.floor(Math.random() * keys.length / 2) * 2 + mode;
  else
    i = Math.floor(Math.random() * keys.length);
  key.value = keys[i];
  playKeyChord(key.value);
  degree.value = null;
  return key.value;
}

function playKeyChord() {
  let ids = [];
  ids.push(getDegree(key.value, 0).noteId);
  ids.push(getDegree(key.value, 2).noteId);
  ids.push(getDegree(key.value, 4).noteId);
  if (ids[1] < ids[0])
    ids[1] += 12;
  if (ids[2] < ids[0])
    ids[2] += 12;
  pressedKeys.value = ids;
  for (let i = 0; i < ids.length; i++) {
    let audio = new Audio(`/sounds/${ids[i]}.mp3`);
    setTimeout(() => {
      audio.play();
    }, i * 500);
  }
}

function pickUpDegree() {
  let num = Math.floor(Math.random() * 7);
  degree.value = { degree: num };
  degree.value.noteId = getDegree(key.value, num).noteId;
  if (Math.random() >= 0.5)
    degree.value.noteId += 12;
  playDegree();
  return degree.value;
}

function playDegree() {
  let audio = new Audio(`/sounds/${degree.value.noteId}.mp3`);
  audio.play();
}

let timeoutCleanResult;
function makeGuess(guess) {
  let result = document.querySelector("#result");
  if (guess === degree.value.degree) {
    window.clearTimeout(timeoutCleanResult);
    result.innerHTML = "Правильно!";
    timeoutCleanResult = setTimeout(() => result.innerHTML = "", 3000);
    pickUpDegree();
  } else {
    window.clearTimeout(timeoutCleanResult);
    result.innerHTML = "Неправильно";
    timeoutCleanResult = setTimeout(() => result.innerHTML = "", 3000);
  }
}
</script>

<style scoped>
button {
  @apply m-1 p-1 border border-black;
}

button:disabled {
  @apply bg-gray-300;
}

.degree-button {
  @apply w-[13.3%] min-w-20;
}

#result {
  @apply text-center;
}

@media (aspect-ratio < 1.2) {
}
</style>
