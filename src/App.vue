<template>
  <div>
    <h1>Morpion</h1>
    <div class="grid">
      <button class="button" v-for="index in refIndex" :key="index" @click="handleButtonClick(index-1)">{{ buttonText[index-1] }}</button>
    </div>
    <h2 v-if="checkWinCondition">{{ messageGagnant }}</h2>
    <button @click="restartGame()">Redémarrer la partie</button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const refIndex = 9;
const buttonCounts = ref(Array(Number(refIndex)).fill(0));
const messageGagnant = ref("C'est à X de jouer !"); 
const player = ref(1);


function handleButtonClick(index) {
  
  if (buttonCounts.value[index] !== 0 || checkWinCondition()) {
    return;
  }

  buttonCounts.value[index] = buttonCounts.value[index] + player.value;
  togglePlayer();

  if (buttonCounts.value[index] > 2) {
    buttonCounts.value[index] = 1;
  }

  if (checkDrawCondition()) {
      messageGagnant.value = "Égalité !";
  }
  if (checkWinCondition()) {
    messageGagnant.value = String(buttonText.value[index]) + ' gagne !';
  }
}

const buttonText = computed(() => {
  return buttonCounts.value.map(count => {
    if (count === 2) {
      return 'O';
    } else if (count === 1) {
      return 'X';
    } else {
      return '';
    }
  });
});

function togglePlayer() {
  player.value = 3 - player.value;
  if (player.value === 1) {
    messageGagnant.value = "C'est à X de jouer !";
  } else if (player.value === 2) {
    messageGagnant.value = "C'est à O de jouer !";
  }
}

function checkWinCondition() {
  for (let i = 0; i < 3; i++) {
    const rowIndex = i * 3;

    if (buttonCounts.value[rowIndex] !== 0 && 
        buttonCounts.value[rowIndex] === buttonCounts.value[rowIndex + 1] && 
        buttonCounts.value[rowIndex + 1] === buttonCounts.value[rowIndex + 2]) {
      return true;
    }
  }

  for (let i = 0; i < 3; i++) {
    const colIndex = i
    if (buttonCounts.value[colIndex] !== 0 && 
        buttonCounts.value[colIndex] === buttonCounts.value[colIndex + 3] && 
        buttonCounts.value[colIndex + 3] === buttonCounts.value[colIndex + 6]) {
      return true;
    }
  }

  if (buttonCounts.value[0] !== 0 && 
      buttonCounts.value[0] === buttonCounts.value[4] && 
      buttonCounts.value[4] === buttonCounts.value[8]) {
    return true;
  }

  if (buttonCounts.value[2] !== 0 && 
      buttonCounts.value[2] === buttonCounts.value[4] && 
      buttonCounts.value[4] === buttonCounts.value[6]) {
    return true;
  }

  return false;
}

function checkDrawCondition() {
  //console.log('buttonCounts.value:', buttonCounts.value);
  return buttonCounts.value.every(count => count !== 0);
}

function restartGame() {
  messageGagnant.value = "C'est à X de jouer !"; 
  player.value = 1;
  for (let i = 0; i <= refIndex-1; i++) {
    if (buttonCounts.value[i] !== 0) {
      buttonCounts.value[i] = 0;
    }
  }
}

</script>