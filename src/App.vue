<script setup lang="ts">
import { ref } from 'vue';

const cells = ref(['', '', '', '', '', '', '', '', '']); // объявляем поля
const currentPlayer = ref('X'); // объявляем текущего игрока
let gameResult = ''; // результат игры

// функция начала игры и перемещение крестика и нолика
const makeMove = (index: number) => {
  if (!gameResult && cells.value[index] === '') {
    cells.value[index] = currentPlayer.value;
    updateGameResult();
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
  }
};
// проверяем на победу 
const isVictory = () => {
  const combs = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (const comb of combs) {
    if (
      cells.value[comb[0]] === cells.value[comb[1]] &&
      cells.value[comb[1]] === cells.value[comb[2]] &&
      cells.value[comb[0]] !== ''
    ) {
      return true;
    }
  }

  return false;
};

// обновляем результаты игры
const updateGameResult = () => {
  if (isVictory()) {
    gameResult = 'Выиграл ' + currentPlayer.value;
  } else if (!cells.value.includes('')) {
    gameResult = 'Ничья';
  } else {
    gameResult = '';
  }
};
// запускаем игру заново после окончания
const gameAgain = () => {
  cells.value = ['', '', '', '', '', '', '', '', ''];
  currentPlayer.value = 'X';
  gameResult = '';
};

</script>

<template>
  <h1>Игра крестики нолики</h1>
  <div class="result" v-text="gameResult"></div>
   <table class="game-field">
        <tr>
          <td v-for="(cell, index) in cells" @click="makeMove(index)" v-text="cell"></td>
        </tr>
      </table>
      <button class="again" @click="gameAgain()">Играть заново</button>
</template>

<style scoped>
.game-field {
   margin: 0 auto;
}
.game-field tr {
  display: grid;
  grid-template-columns: auto auto auto;
  grid-template-rows: auto auto auto ;
}
.game-field td {
  background-color: #fff;
  border: 1px solid #333;
  color: #000;
  width: 48px;
  height: 48px;
  display: flex;
    align-items: center;
    justify-content: center;
} 

h1 {
  font-size: 36px;
}
.again {
  border: none;
  border-radius: 16px;
  color: #000;
  margin-top: 32px;
  transition: ease-out 0.3s;
  background-color: #fff;
}
.again:focus {
  outline: none;
}
.again:focus, 
.again:hover {
  transform: scale(1.05);
}
</style>
