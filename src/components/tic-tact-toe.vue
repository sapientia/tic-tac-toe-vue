<script lang="ts" setup>

import cuadricula from './Cuadricula.vue'
import Header from './Header.vue'
import { ref, shallowRef, watch } from 'vue';

const limit = 9;

const currentInput = ref({ 'inputValue': null, 'inputPosition': null })
const gameFinished = shallowRef(false)

const message = shallowRef('')
const movidas = ref<{ player: string, movimientos: number }[]>([]);


const gridLocation = ref<{ col: number, position: number }>({ col: 0, position: 0 });

const getValue = (value) => {
    currentInput.value.inputValue = value
    incrementsMoves(value)
}

const updatePosition = (position) => {
    gridLocation.value.col = position.col
    gridLocation.value.position = position.position
}

const triggerMessage = (msg) => {
    message.value = msg
}

const checkIfPlayExist = (value) => {
    return movidas.value.find((movida) => movida.player == value)
}

const getPlayerIndex = (value) => {
    return movidas.value.findIndex((movida) => movida.player == value)
}

const incrementsMoves = (value) => {
    if (!movidas.value.length) {
        movidas.value.push({ 'player': value, 'movimientos': 1 });
    } else {
        if (checkIfPlayExist(value)) {
            movidas.value[getPlayerIndex(value)].movimientos++;

        } else {
            movidas.value.push({ player: value, movimientos: 1 });
        }
    }
}


watch(gridLocation, (newValue, oldValue) => {
    if (newValue) {
        console.log(newValue)
        verifyMacth(currentInput.value.inputValue, newValue)

    }
}, { deep: true });

const verifyInputs = (inputs, value) => {
    for (let input of inputs) {
        if (input.value !== value) {
            return false;
        }
    }
    return true;
}

const verifyMacth = (value, position) => {

    const inputsInSameColumn = document.querySelectorAll(`.grid-container[data-column="${position.col}"] input`);
    const inputsInSamePosition = document.querySelectorAll(`.grid-container[data-position="${position.position}"] input`);

    if (verifyInputs(inputsInSameColumn, value) || verifyInputs(inputsInSamePosition, value)) {
        message.value = `It's a match! "${value}" wins!`;
        gameFinished.value = true
    }

}


</script>
<template>
    <div class="container">
        <h1>Tic Tac Toe</h1>
        <br>
        <div class="wrapper-header">
            <Header v-for="movida in movidas" :character="movida.player" :moves="movida.movimientos"></Header>
        </div>
        <div style="color:chartreuse; font-size: 40px;text-align: center;" v-if="message">{{ message }}</div>
        <div class="grid">
            <cuadricula v-for="n in limit" :key="n" @emitValue="getValue" @emitMessage="triggerMessage"
                :column="(n - 1) % 3 + 1" :position="Math.floor((n - 1) / 3) + 1" @emitPosition="updatePosition">
            </cuadricula>
        </div>
        <div v-if="gameFinished" class="wrapper-btn"><button class="btn">Start New Game</button></div>

    </div>
</template>

<style>
.container {
    width: 100%;
    margin: 0 auto;
}

.wrapper-btn {
    width: 100%;
    text-align: center;
    padding: 50px;
}

.btn {
    padding: 10px 50px;
    font-size: 25px;
    border-radius: 10px;
    font-family: 'Pixelify Sans', sans-serif;

}

.container .wrapper-header {
    width: 100%;
    display: flex;
    text-align: center;
}

.grid {
    display: grid;
    width: 70%;
    margin: 0 auto;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    outline: 5px dashed burlywood;
    border-radius: 20px;
}

.grid div {
    padding: 10px;
    border-right: 5px dashed burlywood;
    border-bottom: 5px dashed burlywood;
}
</style>