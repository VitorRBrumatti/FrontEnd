<template>
    <div class="back" v-if="showSubTaskModal">
        <div class="sub-tarefa">
            <div class="superior">  
                <div style="display: flex; align-items: center; justify-content: space-between;">
                    <input class="name-sub-task" type="text" v-model="nameSubTask" placeholder="Nome da Tarefa">
                    <span v-if="errors.title_subtask" style="color: #ff0000; font-size: 14px;">{{ errors.title_subtask[0] }}</span>
                </div>
                <div style="display: flex; align-items: center; justify-content: space-between;">
                    <input class="description" type="text" v-model="descriptionSubTask" placeholder="Descrição">
                    <span v-if="errors.description_subtask" style="color: #ff0000; font-size: 14px;">{{ errors.description_subtask[0]
                        }}</span>
                </div>
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
    data() {
        return {
            errors: [],
        }
    },
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
            if (this.selectedSubTask.id) {
                axios.put(`/subtask/${this.selectedSubTask.id}`, data)
                      .then(() => this.$emit('update:showSubTaskModal', false))
                      .then(() => this.$emit('Visualization'))
                      .then(() => this.$emit('update-get'))
                      .catch(e => {
                          this.errors = e.response.data
                     });
            } else {
                axios.post('/subtask', data)
                      .then(() => this.$emit('update:showSubTaskModal', false))
                      .then(() => this.$emit('Visualization'))
                      .then(() => this.$emit('update-get'))
                      .catch(e => {
                          this.errors = e.response.data
                     });
            }
           
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
        selectedSubTask: {
            type: Object,
            required: false,
        }
    },
    watch: {
        showSubTaskModal(newValue, oldValue) {
            if (newValue === true && this.selectedSubTask.id) {
                this.nameSubTask = this.selectedSubTask.title_subtask;
                this.descriptionSubTask = this.selectedSubTask.description_subtask;
            } else if (newValue === true) {
                this.nameSubTask = '';
                this.descriptionSubTask = '';
            }
        }
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

.sub-tarefa {
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

.name-sub-task {
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

.name-sub-task,
.description {
    border: none;
    outline: none;
    width: 325px;
    height: 15px;
    color: #81858E;
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