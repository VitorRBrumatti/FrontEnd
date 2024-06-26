<template>
    <div class="background" v-if="showVisualize">
        <div class="task-visualizer">
            <div class="top">
                <div class="menu-task" v-show="showMenu === true">
                    <div class="copy-link">
                        <img src="../assets/copy-icon.svg" alt="copy-icon">
                        <p>Copiar Link da tarefa</p>
                    </div>
                    <div class="duplicate">
                        <img src="../assets/duplicate-icon.svg" alt="duplicate-icon">
                        <p>Duplicar tarefa</p>
                    </div>
                    <div class="impress">
                        <img src="../assets/impress-icon.svg" alt="impress-icon">
                        <p>Imprimir tarefa</p>
                    </div>
                    <div class="menu-delete" @click="deleteMenu(selectedTask)">
                        <img src="../assets/delete-red.svg" alt="delete-red">
                        <p>Excluir tarefa</p>
                    </div>
                </div>
                <div class="date" :class="{
                    'on-time': !isPastDue,
                    'expired': isPastDue
                }">
                    <img src="../assets/calendar-valid.svg" v-if="!isPastDue">
                    <img src="../assets/calendar-expired.svg" v-if="isPastDue">
                    <p>{{ !isPastDue ? "No prazo" : "Expirado" }}</p>
                </div>
                <div class="icons">
                    <img class="tree-points" src="../assets/tree-points.svg" alt="tree-points" @click="openMenu">
                    <img class="close-icon" src="../assets/close-icon.svg" alt="close-icon" @click="closeVisualize()">
                </div>
            </div>
            <div class="bottom">
                <div class="task-infos">
                    <input type="checkbox" name="checkbox" id="custom-checkbox" v-model="IsTaskChecked"
                        @change="updateTaskStatus" />
                    <label for="custom-checkbox"></label>
                    <span class="title">{{ selectedTask.title }}</span>
                    <p class="description">{{ selectedTask.description }}</p>

                    <div class="subtask">
                        <div class="title-section">
                            <span>Subtarefas</span>
                        </div>
                        <div class="subtask-organize">

                            <div class="show-subtask" v-for="subtask in selectedTask.subtasks"
                                :key="selectedTask.subtasks.id">
                                <div>
                                    <input type="checkbox" :name="'checkbox' + subtask.id"
                                        :id="'custom-checkbox' + selectedTask.id + '_' + subtask.id"
                                        v-model="IsTaskChecked" @change="updateTaskStatus" />
                                    <label :for="'custom-checkbox' + selectedTask.id + '_' + subtask.id"></label>
                                    <span class="subtask-title">{{ subtask.title_subtask }}</span>
                                </div>

                                <div class="hover-icons">
                                    <div class="hovered-edit">
                                        <span id="edit-tooltip">Editar tarefa</span>
                                        <img src="../assets/edit-icon.svg" alt="edit-icon" @click="openCreateSubTask(subtask)"
                                            @click.stop="''">
                                    </div>
                                    <div class="hovered-due-date">
                                        <span id="date-tooltip">Definir vencimento</span>
                                        <img src="../assets/set-due-date.svg" alt="set-due-date" @click.stop="''">
                                    </div>
                                    <div class="hovered-delete">
                                        <img src="../assets/delete-icon.svg" alt="delete-icon"
                                            @click="deleteSubTask(subtask)" @click.stop="''">
                                        <span id="delete-tooltip">Excluir tarefa</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <button class="create-subtask" @click="openCreateSubTask()">Criar Subtarefas</button>
                    </div>
                </div>
                <div class="lateral-infos">
                    <div>
                        <p>Criado em</p>
                        <div class="created">
                            <img src="../assets/calendar-black.svg" alt="calendar-black">
                            <span> {{ formattedCreatedAt }}</span>
                        </div>
                    </div>
                    <div>
                        <p>Data de Vencimento</p>
                        <div class="validate-date">
                            <img src="../assets/calendar-valid.svg" alt="calendar-valid" v-if="!isPastDue">
                            <img src="../assets/calendar-expired.svg" alt="calendar-expired" v-if="isPastDue">
                            <span :style="{ color: !isPastDue ? '#009488' : '#D31408' }">{{ formattedDueDate }}</span>
                        </div>
                    </div>
                    <div>
                        <p>Modificado em</p>
                        <div class="updated">
                            <img src="../assets/calendar-black.svg" alt="calendar-black">
                            <span>{{ formattedUpdatedAt }}</span>
                        </div>
                    </div>
                    <div class="id-task">
                        <p>ID da tarefa</p>
                        <span>{{ selectedTask.id }}</span>
                    </div>
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
            showMenu: false,
        }
    },
    props: {
        showVisualize: {
            type: Boolean,
            required: true,
        },
        selectedTask: {
            type: Object,
            required: false,
        },
    },
    computed: {

        isToday() {
            let today = new Date();
            let taskDate = new Date(this.selectedTask.due_date);
            return taskDate.getFullYear() === today.getFullYear() &&
                taskDate.getMonth() === today.getMonth() &&
                taskDate.getDate() === today.getDate();
        },
        isPastDue() {
            let today = new Date();
            let taskDate = new Date(this.selectedTask.due_date);
            return taskDate < today && !this.isToday;
        },
        formattedDueDate() {
            let dateObject = new Date(this.selectedTask.due_date);
            let day = dateObject.getDate().toString().padStart(2, '0');
            let month = (dateObject.getMonth() + 1).toString().padStart(2, '0');
            let year = dateObject.getFullYear();
            return `${day}/${month}/${year}`;
        },
        formattedCreatedAt() {
            let date = new Date(this.selectedTask.created_at);
            let day = date.getDate().toString().padStart(2, '0');
            let month = (date.getMonth() + 1).toString().padStart(2, '0');
            let year = date.getFullYear();
            let hours = date.getHours().toString().padStart(2, '0');
            let minutes = date.getMinutes().toString().padStart(2, '0');
            return `${day}/${month}/${year} às ${hours}:${minutes}`;
        },
        formattedUpdatedAt() {
            let date = new Date(this.selectedTask.updated_at);
            let day = date.getDate().toString().padStart(2, '0');
            let month = (date.getMonth() + 1).toString().padStart(2, '0');
            let year = date.getFullYear();
            let hours = date.getHours().toString().padStart(2, '0');
            let minutes = date.getMinutes().toString().padStart(2, '0');
            return `${day}/${month}/${year} às ${hours}:${minutes}`;
        },
        IsTaskChecked: {
            get() {
                return this.selectedTask && this.selectedTask.status === 'completed';
            },
            set(value) {
                this.$emit('update-task-status', { taskId: this.selectedTask.id, newStatus: value ? 'completed' : 'pending' });
            }
        }

    },
    IsTaskChecked: {
        get() {
            return this.selectedTask.status === 'completed';
        },
        set(value) {
            this.$emit('update-task-status', { taskId: this.selectedTask.id, newStatus: value ? 'completed' : 'pending' });
        }
    },
    methods: {
        closeVisualize() {
            this.$emit('update:showVisualize', false);
        },
        openMenu() {
            if (this.showMenu === true) {
                this.showMenu = false
            }
            else {
                this.showMenu = true;
            }
        },
        deleteMenu(selectedTask) {
            axios.delete(`/task/${selectedTask.id}`)
                .then(() => this.$emit('update:showVisualize', false))
                .then(() => this.$emit('update-get'));
        },
        openCreateSubTask(subtask) {
            this.$emit('show-Sub-Task', subtask);
        },
        deleteSubTask(subtask) {
            axios.delete(`/subtask/${subtask.id}`)
                .then(() => {
                    this.selectedTask.subtasks = this.selectedTask.subtasks.filter(item => item.id !== subtask.id)
                        .then(() => this.$emit('update-get'));
                })
                .catch(error => {
                    alert("erro encontrado! Tente novamente mais tarde")
                });
        },
    },

}
</script>

<style scoped>
* {
    font-family: Montserrat;
}

.background {
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

.bottom input[type="checkbox"] {
    display: none;

}

.bottom span {
    color: #000000;
    font-weight: 500;
}

.bottom label::before {
    content: '';
    width: 18px;
    height: 18px;
    border-radius: 50%;
    border: 1px solid rgb(0, 0, 0);
    display: inline-block;
    vertical-align: middle;
    margin-right: 23px;
    margin-bottom: 3px;
    transition: background-color 0.3s ease;
}

.bottom input[type="checkbox"]:checked+label::before {
    background-color: rgb(0, 0, 0);
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='18' height='18' viewBox='0 0 10 10'%3E%3Cg class='nc-icon-wrapper' stroke-width='1' fill='%23555555'%3E%3Cpath fill='none' stroke='%23FFFFFF' stroke-linecap='round' stroke-linejoin='round' stroke-miterlimit='10' data-cap='butt' d='M2.83 4.72l1.58 1.58 2.83-2.83'/%3E%3C/g%3E%3C/svg%3E");
    background-position: center;
    border: none;
    padding: 1px;
    transition: background-color 0.3s ease;
}

.task-visualizer {
    width: 819px;
    height: 613px;
    background: #FFFFFF;
    box-shadow: 2px 4px 10px 0px #0000000D;
    overflow: hidden;
    display: flex;
    flex-direction: column;
}

.top {
    display: flex;
    height: 70px;
    justify-content: space-between;
    align-items: center;
}

.bottom {
    border-top: 1px solid #D9D9D9;
    color: #000000;
    display: flex;
    height: 100%;
}

.task-infos {
    margin: 30px;
    width: 510px;
    height: 80px;

}

.title {
    font-size: 18px;
    font-weight: 600;
    line-height: 21.94px;
    width: 50px;
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: clip;
}

.description {
    color: #81858E;
    margin-left: 44px;
    font-size: 14px;
    font-weight: 400;
    line-height: 21px;
    width: 90%;
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: clip;
}

.date {
    display: flex;
    margin-left: 30px;
}

.icons {
    display: flex;
    gap: 50px;
    margin-right: 30px;
}

.close-icon {
    cursor: pointer;
}

.tree-points {
    cursor: pointer;
}

.title-section {
    margin-top: 30px;
    height: 40px;
    border-bottom: 1px solid #E5E5E5;
    font-size: 14px;
    font-weight: 600;
    line-height: 17.07px;
    text-align: left;

}

.title-section span {
    font-size: 14px;
    font-weight: 600;

}

.expired {
    background: #D314081A;
    color: #D31408 !important;
    font-weight: 500;
    width: 110px;
    height: 30px;
    display: flex;
    top: 5px;
    cursor: default;
}

.expired img {
    width: 13;
    height: 14;
    top: -6px;
    left: 5px;
    margin-top: 12px;
    position: relative;
}

.expired p {
    color: #D31408;
    position: relative;
    left: 16px;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    top: 4px;
}

.on-time {
    background: #0094881A;
    color: #009488 !important;
    font-weight: 500;
    width: 110px;
    height: 30px;
    display: flex;
    cursor: default;
}

.on-time img {
    width: 13;
    height: 14;
    top: -6px;
    left: 5px;
    margin-top: 12px;
    position: relative;

}

.on-time p {
    color: #009488;
    position: relative;
    left: 13px;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    top: 4px;
}

.lateral-infos {
    position: relative;
    background-color: #F7F7F7;
    width: 246px;
    height: 100%;
    align-self: flex-end;
    left: 3px;
    display: flex;
    flex-direction: column;
    gap: 40px;
    padding-left: 30px;
    padding-top: 40px;
}

.updated {
    color: #000000;
    display: flex;
    align-items: center;
    gap: 10px;
}

.created {
    color: #000000;
    display: flex;
    align-items: center;
    gap: 10px;
}

.created span,
.updated span,
.id-task span {
    font-size: 14px;
    font-weight: 600;
    line-height: 17.07px;
}

.validate-date {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 14px;
    font-weight: 600;
    line-height: 17.07px;
}

.validate-date span {
    align-items: center;
    font-size: 14px;
    font-weight: 600;
    line-height: 17.07px;
}

.menu-task {
    position: absolute;
    background-color: #FFFFFF;
    width: 246px;
    height: 212px;
    right: 27%;
    top: 22%;
    color: #000000;
    z-index: 1;
    box-shadow: 2px 4px 10px 0px #0000000D;
    display: flex;
    flex-direction: column;
    gap: 30px;
}

.copy-link {
    margin-top: 20px;
}

.copy-link,
.duplicate,
.impress,
.menu-delete {
    display: flex;
    gap: 20px;
    margin-left: 20px;
    font-size: 14px;
    font-weight: 400;
    line-height: 17.07px;
    text-align: left;
    cursor: pointer;
}

.menu-delete p {
    color: #D31408;
}

.subtask-organize {
    display: flex;
    flex-direction: column;
    gap: 15px;
    height: 260px;
    margin-top: 20px;
    margin-bottom: 20px;
    overflow: auto;
}

.show-subtask {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.show-subtask:hover .hover-icons {
    opacity: 1;
}

.hovered-edit:hover #edit-tooltip,
.hovered-due-date:hover #date-tooltip,
.hovered-delete:hover #delete-tooltip {
    opacity: 1;
    visibility: visible;
    transition-delay: 0s;

}

.hovered-delete {
    width: 19px;
}

#edit-tooltip {
    position: absolute;
    right: 90%;
    bottom: 120%;
    background-color: #000000CC;
    color: #FFFFFF;
    padding: 5px;
    white-space: nowrap;
    font-size: 13px;
    font-weight: 500;
    line-height: 15.85px;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s ease, visibility 0s linear 0.3s;

}

#date-tooltip {
    position: absolute;
    font-size: 13px;
    font-weight: 500;
    line-height: 15.85px;
    right: 50%;
    bottom: 130%;
    background-color: #000000CC;
    color: #FFFFFF;
    padding: 5px;
    white-space: nowrap;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s ease, visibility 0s linear 0.3s;
}

#delete-tooltip {
    position: absolute;
    right: 8%;
    bottom: 130%;
    background-color: #000000CC;
    color: #FFFFFF;
    padding: 5px;
    white-space: nowrap;
    font-size: 13px;
    font-weight: 500;
    line-height: 15.85px;
    opacity: 1;
    visibility: hidden;
    transition: opacity 0.3s ease, visibility 0s linear 0.3s;
}

.hover-icons {
    display: flex;
    gap: 25px;
    height: 20px;
    opacity: 0;
    position: relative;
    margin-right: 25px;
    transition: 0.3s opacity ease;
}

.hover-icons img {
    display: flex;
    cursor: pointer;
}
</style>