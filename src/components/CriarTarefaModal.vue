
<template>
    <div class="backdrop" v-if="showModal">
        <div class="nova-tarefa">
            <div class="superior">
                <input class="nameTask" type="text" v-model="nameTask" placeholder="Nome da tarefa">
                <input class="description" type="text" v-model="descriptionTask" placeholder="Descrição">
                <input class="date" v-model="taskExpire" type="date">
            </div>
            <div class="inferior">
                <div class="buttons">
                    <button class="cancelar" @click="closeModal()" >Cancelar</button>
                    <button class="criar" @click="createTask()">Criar Tarefa</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    props: {
        showModal: {
            type: Boolean,
            required: true,
        },
    },
    methods: {
        closeModal() {
            this.$emit('update:showModal', false);
        },
        createTask() {
            let data = {
                title: this.nameTask,
                description: this.descriptionTask,
                due_date: this.taskExpire
            }
            console.log(data);
            // axios.post('tarefa', data)
            // .then(() => this.$emit('update:showModal', false))
        }
    },
}
</script>


<style scoped>
.backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    overflow: hidden;
    background-color: #0005;
    display: flex;
    justify-content: center;
    align-items: center;
} 

.nova-tarefa {
    width: 678px;
    height: 216px;
    background: #FFFFFF;
    box-shadow: 2px 4px 10px 0px #0000000D;
    overflow: hidden;
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