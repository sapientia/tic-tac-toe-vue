<script setup>
import { useTemplateRef, computed } from 'vue';


const props = defineProps({
    character: String,
    position: Number,
    column: Number,
})

const emit = defineEmits(['emitValue', 'emitMessage', 'emitPosition'])

const inputRef = useTemplateRef()
const grid = defineModel()



const isDisabled = computed(() => {
    return grid.value && grid.value.trim() !== '';
});

const position = {
    'col': props.column,
    'position': props.position
}

const validateInput = (value) => {
    const normalizedValue = value.toLowerCase()
    if (normalizedValue === 'x' || normalizedValue === '0') {
        emit('emitValue', normalizedValue);
        emit('emitMessage', null)
        emit('emitPosition', position)
    } else {
        emit('emitMessage', 'Allowed values are X OR 0, please try again')
        grid.value = null;
    }
};

</script>

<template>
    <div class="grid-container" :data-position="props.position" :data-column="props.column">
        <input ref="inputRef" v-model="grid" @input="validateInput($event.target.value)" :disabled="isDisabled"
            placeholder="x/0" />
    </div>
</template>

<style>
input {
    font-size: 80px;
    height: 150px;
    width: 100%;
    font-weight: bold;
    border: none;
    background: none;
    text-align: center;
    color: white;
    font-family: 'Pixelify Sans', sans-serif;
    text-align: center;
}

input:focus {
    border: none
}

input::placeholder {
    color: #582c85;
}
</style>