<script setup>
   import Beleza from '../assets/beleza.png'
</script>

<template>
    <div class="detail" v-on:mouseover="colorChange()">
        <beleza></beleza>
    </div>
</template>

<style scoped>
    p {
    position: relative;
    width: 48px;
    height: 48px;
    left: 96px;
    top: 91px;
}

.detail {
    width: 609px;
    height: 599px;
    top: 31px;
    left: 32px;
    gap: 0px;
    background: #F7F7F7;
}

.detail:hover {
    transition: 0.4s;
    background: black;
}
</style>

