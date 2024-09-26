<script setup>

const props = defineProps({
    character: String,
    moves: Number,
})
</script>

<template>
    <div class="header">
        <h2> Player {{ character }}</h2>
        <h3>moves: {{ moves }}</h3>
    </div>

</template>

<style>
.header {
    width: 50%;
}
</style>