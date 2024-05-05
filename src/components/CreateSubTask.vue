<template>
    <div class="back" v-if="showSubTaskModal">
        <div class="Subtarefa">
            <div class="superior">
                <input class="nameTask" type="text" v-model="nameSubTask" placeholder="Nome da Subtarefa">
                <input class="description" type="text" v-model="descriptionSubTask" placeholder="Descrição">
            </div>
            <div class="inferior">
                <div class="buttons">
                    <button class="cancelar" @click="closeModal()">Cancelar</button>
                    <button class="criar" @click="createSubTask()">Criar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    methods: {
        closeModal() {
            this.$emit('update:showSubTaskModal', false);
        },
        createSubTask() {
            let data = {
                title_subtask: this.nameSubTask,
                task_id: this.selectedTask.id,
                description_subtask: this.descriptionSubTask
            }
            axios.post('/Subtask', data)
                .then(() => this.$emit('update:showSubTaskModal', false));
        }
    },
    props: {
        showSubTaskModal: {
            type: Boolean,
            required: true
        },
        selectedTask: {
            type: Object,
            required: false,
        },
    },
}
</script>

<style scoped>
.back {
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

.Subtarefa {
    width: 678px;
    height: 216px;
    background: #FFFFFF;
    box-shadow: 2px 4px 10px 0px #0000000D;
    overflow: hidden;
}

.superior {
    display: flex;
    flex-direction: column;
    padding: 20px;
    width: 678px;
    height: 148px;
    gap: 40px;
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