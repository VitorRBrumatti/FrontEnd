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
            <button>Cancelar</button>
            <button>Criar Tarefa</button>
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
    gap: 0px;
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
    background-image: url('../assets/calendar.svg');
    background-repeat: no-repeat;
    background-size: 20px 20px;
    background-position: left center;
    padding-left: 8%;
}
input.date::-webkit-calendar-picker-indicator {
    display: none;

}
</style>