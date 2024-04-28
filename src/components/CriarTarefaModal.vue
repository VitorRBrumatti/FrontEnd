<script>

export default {

}
</script>

<template>
    <div class="nova-tarefa">
        <div class="superior">
            <input class="nameTask" type="text" vmodel="nameTask" placeholder="Nome da tarefa">
            <input class="description" type="text" vmodel="description" placeholder="Descrição">
            <input class="date" vmodel="date" type="date">
        </div>
        <div class="inferior">
            <div class="buttons">
                <button class="cancelar">Cancelar</button>
                <button class="criar">Criar Tarefa</button>
            </div>
        </div>
    </div>
</template>


<style scoped>
.nova-tarefa {
    width: 678px;
    height: 216px;
    background: #FFFFFF;
    border: 1px solid #E5E5E5;
    box-shadow: 2px 4px 10px 0px #0000000D;
}

.superior {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 20px;
    width: 678px;
    height: 148px;
    gap: 20px;
    border: 1px solid #E5E5E5
}

.inferior {
    border: 1px solid #E5E5E5;
    border-top: none;
    width: 678px;
    height: 68px;
    top: 148px;
    display: flex;
    align-items: center;
    justify-content: end;
    padding-right: 30px;
}

.nameTask {
    font-family: Montserrat;
    font-size: 16px;
    font-weight: 400;
    line-height: 19.5px;
}

.description {
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 400;
    line-height: 17.07px;
}

.nameTask,
.description {
    border: none;
    outline: none;
    width: 447px;
    height: 17px;

}

.date {
    width: 199px;
    height: 40px;
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 400;
    line-height: 17.07px;
    border: 1px solid #E5E5E5;
}

input.date {
    font-family: Montserrat;
    color: #E5E5E5;
    background-repeat: no-repeat;
    background-size: 20px 20px;
    background-position: left center;
    padding-left: 8%;
    padding-right: 20px;
    cursor: pointer;
}

input.date::-webkit-calendar-picker-indicator {
    position: relative;
    right: 115%;
    width: 20px;
    background-size: 20px 20px;
    color: #E5E5E5;
    background-position: center;
    background-image: url('../assets/calendar.svg');
}

.buttons {
    display: flex;
    gap: 20px;
}
.criar {
    cursor: pointer;
    width: 122px;
    height: 40px;
    background: #000000;
    color: #FFFFFF;
    border: none;
}

.cancelar {
    cursor: pointer;
    width: 122px;
    height: 40px;
    background: #F7F7F7;
    color: #000000;
    border: none;
}
</style>