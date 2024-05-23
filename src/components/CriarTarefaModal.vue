<template>
    <div class="backdrop" v-if="showModal">
        <div class="new-task">
            <div class="superior">
                <div style="display: flex; align-items: center; justify-content: space-between;">
                    <input class="name-task" type="text" v-model="nameTask" placeholder="Nome da Tarefa">
                    <span v-if="errors.title" style="color: #ff0000; font-size: 14px;">{{ errors.title[0] }}</span>
                </div>
                <div style="display: flex; align-items: center; justify-content: space-between;">
                    <input class="description" type="text" v-model="descriptionTask" placeholder="Descrição">
                    <span v-if="errors.description" style="color: #ff0000; font-size: 14px;">{{ errors.description[0]
                        }}</span>
                </div>

                <div style="display: flex; gap: 20px; align-items: center; justify-content: space-between;">
                    <input class="date" v-model="taskExpire" type="date">
                    <span v-if="errors.due_date" style="color: #ff0000; font-size: 14px;">{{ errors.due_date[0]
                        }}</span>
                </div>

            </div>
            <div class="bottom">
                <div class="buttons">
                    <button class="cancelar" @click="closeModal()">Cancelar</button>
                    <button class="create" @click="createTask()">{{ selectedTask.id ? 'Editar' : 'Criar' }}
                        Tarefa</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            nameTask: '',
            descriptionTask: '',
            taskExpire: '',
            errors: [],
        }
    },
    props: {
        showModal: {
            type: Boolean,
            required: true,
        },
        selectedTask: {
            type: Object,
            required: false,
        }

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
            if (this.selectedTask.id) {
                axios.put(`/task/${this.selectedTask.id}`, data)
                    .then(() => this.$emit('update:showModal', false))
                    .then(() => this.$emit('update-get'))
                    .catch(e => {
                        this.errors = e.response.data
                    });
            }
            else {
                axios.post('/task', data)
                    .then(() => this.$emit('update:showModal', false))
                    .then(() => this.$emit('update-get'))
                    .catch(e => {
                        this.errors = e.response.data
                    });
            }

        }
    },
    watch: {
        showModal(newValue, oldValue) {
            if (newValue === true && this.selectedTask.id) {
                this.nameTask = this.selectedTask.title;
                this.descriptionTask = this.selectedTask.description;
                this.taskExpire = new Date(this.selectedTask.due_date).toISOString().substr(0, 10);
            } else if (newValue === true) {
                this.nameTask = '';
                this.descriptionTask = '';
                this.taskExpire = '';
            }
        }
    }
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

.new-task {
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
    gap: 15px;
    border: 1px solid #E5E5E5
}

.bottom {
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

.name-task {
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

.name-task,
.description {
    border: none;
    outline: none;
    width: 325px;
    height: 15px;
    color: #81858E;


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
    color: #81858E;
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
    background-size: 20px 18px;
    color: #81858E;
    background-position: center;
    background-image: url('../assets/calendar.svg');
}

.buttons {
    display: flex;
    gap: 20px;
}

.create {
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