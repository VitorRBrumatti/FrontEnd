<script>

</script>

<template>

    <div class="container">
        <div class="efect">
            <div class="detail"></div>
            <img src="../assets/camada.svg" alt="Camadas" class="camadas">
            <h1 class="title">A fortuna fica ao lado daquele que ousa.</h1>
            <p class="sub-title">Todo progresso ocorre fora da zona de conforto.</p>
        </div>
        <img src="../assets/rockLoose.png" alt="RockLoose" class="rock-and-roll">  
    </div>
    
</template>

<style scoped>
.container {
    position: relative;
}
.efect:hover {
    transition: 0.3s;
    filter: invert(1);
}
.rock-and-roll {
    font-size: 48px;
    font-family: Montserrat;
    position: absolute;
    width: 48px;
    height: 48px;
    top: 9%;
    left: 10%;
}

.title {
    color: black;
    position: absolute;
    top: 40%;
    font-family: Montserrat;
    font-size: 24px;
    font-weight: 700;
    line-height: 36px;
    padding-left: 39px;
}

.sub-title {
    color: #000000;
    position: absolute;
    top: 49%;
    width: 513px;
    height: 24px;
    gap: 0px;
    opacity: 0px;
    font-family: Montserrat;
    font-size: 16px;
    font-weight: 400;
    line-height: 24px;
    text-align: left;
    padding-left: 39px;


}

.camadas {
    position: absolute;
    width: 235px;
    height: 100px;
    top: 459px;
    left: 76px;
}

.detail {
    width: 609px;
    height: 599px;
    top: 31px;
    left: 32px;
    gap: 0px;
    background: #F7F7F7;
}
</style>
