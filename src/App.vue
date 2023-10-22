<template>
    <div class="container">
        <h1 class="title">Tic-Tac-Toe</h1>
        <h2 v-if="!winner" class="sub-title">Its '{{ symbol }}' turn</h2>
        <h2 v-else-if="winner === 'X' || winner === 'O'" class="winner-title">The Winner is {{ winner }}</h2>
        <h2 v-else class="no-winner-title">There is {{ winner }}</h2>
        <button 
            v-if="winner" 
            class="reset-btn"
            @click="resetGame"
        >
            New Game
        </button>
        <div class="game">
            <Box 
                v-for="(cell, index) in allCells" 
                :key="index" 
                :turn="cell" 
                :winner="winner" 
                @set-symbol="handleTurn(index + 1)" 
            />
        </div>
    </div>
</template>
 
<script setup>
import Box from './components/Box.vue';
import { reactive, ref, computed } from 'vue';


const turn = ref(false);
const turnCount = ref(0);
const winner = ref(null)
const reset = ref(false);
const gridTurns = ref([['','',''],['','',''],['','','']])
const rowReference = {
    1 : 0,
    2 : 0,
    3 : 0,
    4 : 1,
    5 : 1,
    6 : 1,
    7 : 2,
    8 : 2,
    9 : 2
}

const colReference = {
    1: 0,
    4: 0,
    7: 0,
    2: 1,
    5: 1,
    8: 1,
    3: 2,
    6: 2,
    9: 2
}

const handleTurn = (cell) =>{
    if (turnCount.value < 10 && !winner.value ) {
        const row = rowReference[cell]
        const col = colReference[cell]
        if (gridTurns.value[row][col] === '' ) {
            gridTurns.value[row][col] = turn.value ? 'X' : 'O';
            turn.value = !turn.value;
            turnCount.value++;
        }
        winner.value = verifyWinner()
    }

}


const verifyWinner = () =>{

    // Check rows
    for (let i = 0; i < 3; i++) {
        if (gridTurns.value[i][0] !== '' && gridTurns.value[i][0] === gridTurns.value[i][1] && gridTurns.value[i][1] === gridTurns.value[i][2]) {
            return gridTurns.value[i][0];
        }
    }
    
    // Check columns
    for (let j = 0; j < 3; j++) {
        if (gridTurns.value[0][j] !== '' && gridTurns.value[0][j] === gridTurns.value[1][j] && gridTurns.value[1][j] === gridTurns.value[2][j]) {
            return gridTurns.value[0][j];
        }
    }
    
    // Check diagonals
    if (gridTurns.value[0][0] !== '' && gridTurns.value[0][0] === gridTurns.value[1][1] && gridTurns.value[1][1] === gridTurns.value[2][2]) {
        return gridTurns.value[0][0];
    }
    if (gridTurns.value[0][2] !== '' && gridTurns.value[0][2] === gridTurns.value[1][1] && gridTurns.value[1][1] === gridTurns.value[2][0]) {
        return gridTurns.value[0][2];
    }
    
    if (turnCount.value >= 9) {
        return "No winner"
    }
}

const resetGame = () => {
    for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
            gridTurns.value[i][j] = '';
        }
    }
    turn.value = false;
    turnCount.value = 0;
    winner.value = null;
    console.log(gridTurns.value)
}

const symbol = computed(() => turn.value ? 'X' : 'O')

const allCells = computed(() => {
    const cells = [...gridTurns.value[0],...gridTurns.value[1],...gridTurns.value[2]]
    console.log(cells)
    return cells
    })
</script>

<style scoped>
.container{
    @apply w-screen h-screen flex flex-col justify-center items-center gap-8 mx-auto;
}

.title{
    @apply text-3xl text-white text-center;
}

.sub-title{
    @apply text-2xl text-white text-center;
}

.no-winner-title{
    @apply font-bold text-4xl text-white text-center;
}

.winner-title{
    @apply font-bold text-6xl text-white text-center;
}

.game{
    @apply grid grid-cols-3 grid-rows-3;
}

.reset-btn{
    @apply bg-blue-500 hover:bg-blue-700 px-4 py-2 rounded-md;
}
</style>
