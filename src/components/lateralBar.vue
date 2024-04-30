<script>

</script>
<template>
    <div class="lateralOptions">
        <div class="icons">
            <div class="icon-organize">
                <img class="img-entrada" src="../assets/entrada-icon.svg" alt="Entrada icon">
                <p>Entrada</p>
            </div>
            <div class="icon-organize">
                <img class="calendar-hoje" src="../assets/calendar-lateral.svg" alt="calendar">
                <p>Tarefas de hoje</p>
            </div>
            <div class="icon-organize">
                <img class="warning" src="../assets/warning-icon.svg" alt="Warning Icon">
                <p>Vencidos</p>
            </div>
        </div>
    </div>
</template>

<style scoped>
.lateralOptions {
    position: absolute;
    width: 293px;
    height: calc(100vh - 80px);
    margin-top: 80px;
    left: 0;
    background-color: #FAFAFA;
    align-self: self-start;
    display: flex;
}

.icons {
    width: 231px;
    height: 159.1px;
    margin: 40px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.icon-organize {
    display: flex;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 600;
    line-height: 18.29px;
    text-align: left;
    color: #000000;
}
p {
    padding-left: 20px;
}
</style>