<template>
    <div class="background" v-if="showVisualize">
        <div class="task-visualizer">
            <div class="top">
                <div class="date" :class="{
                    'on-time': !isPastDue(selectedTask.due_date),
                    'expired': isPastDue(selectedTask.due_date)
                }">
                <div class="menu-task">
                    <div>Copiar Link</div>
                    <div>Duplicar tarefa</div>
                    <div>Imprimir</div>
                    <div>Excluir</div>
                </div>
                    <img src="../assets/calendar-valid.svg" v-if="!isPastDue(selectedTask.due_date)">
                    <img src="../assets/calendar-expired.svg" v-if="isPastDue(selectedTask.due_date)">
                    <p>{{ !isPastDue(selectedTask.due_date) ? "No prazo" : "Expirado" }}</p>
                </div>
                <div class="icons">
                    <img class="tree-points" src="../assets/tree-points.svg" alt="tree-points">
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

                    </div>
                </div>
                <div class="lateralInfos">
                    <div>
                        <p>Criado em</p>
                        <div class="created">
                            <img src="../assets/calendar-black.svg" alt="calendar-black">
                            <span> {{ formatCreatedAt(selectedTask.created_at) }}</span>
                        </div>
                    </div>
                    <div>
                        <p>Data de Vencimento</p>
                        <div class="validate-date">
                            <img src="../assets/calendar-valid.svg" alt="calendar-valid"
                                v-if="!isPastDue(selectedTask.due_date)">
                            <img src="../assets/calendar-expired.svg" alt="calendar-expired"
                                v-if="isPastDue(selectedTask.due_date)">
                            <span :style="{ color: !isPastDue(selectedTask.due_date) ? '#009488' : '#D31408' }">{{
                                formatDueDate(selectedTask.due_date) }}</span>
                        </div>
                    </div>
                    <div>
                        <p>Modificado em</p>
                        <div class="updated">
                            <img src="../assets/calendar-black.svg" alt="calendar-black">
                            <span>{{ formatCreatedAt(selectedTask.updated_at) }}</span>
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
export default {
    data() {
        return {
            nameTask: '',
            descriptionTask: '',
            taskExpire: '',
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
        IsTaskChecked: {
            get() {
                return this.selectedTask.status === 'completed';
            },
            set(value) {
                this.$emit('update-task-status', { taskId: this.selectedTask.id, newStatus: value ? 'completed' : 'pending' });
            }
        }
    },
    methods: {
        closeVisualize() {
            this.$emit('update:showVisualize', false);
        },
        isToday(date) {
            let today = new Date();
            let taskDate = new Date(date);
            return taskDate.getFullYear() === today.getFullYear() &&
                taskDate.getMonth() === today.getMonth() &&
                taskDate.getDate() === today.getDate();
        },
        isPastDue(date) {
            let today = new Date();
            let taskDate = new Date(date);
            return taskDate < today && !this.isToday(date);
        },
        updateTaskStatus() {
            this.isChecked = !this.isChecked;
        },
        formatDueDate(dueDate) {
            let dateObject = new Date(dueDate);
            let day = dateObject.getDate().toString().padStart(2, '0');
            let month = (dateObject.getMonth() + 1).toString().padStart(2, '0');
            let year = dateObject.getFullYear();
            return `${day}/${month}/${year}`;
        },
        formatCreatedAt(createdAt) {
            let created_at = createdAt;
            let date = new Date(created_at);
            let day = date.getDate();
            let month = date.getMonth() + 1;
            let year = date.getFullYear();
            let hours = date.getHours();
            let minutes = date.getMinutes();
            let formatted_date = (day < 10 ? '0' : '') + day + '/' + (month < 10 ? '0' : '') + month + '/' + year + ' às ' + (hours < 10 ? '0' : '') + hours + ':' + (minutes < 10 ? '0' : '') + minutes;
            return formatted_date;
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

.bottom label:before {
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

.bottom input[type="checkbox"]:checked+label:before {
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

}

.description {
    color: #81858E;
    margin-left: 44px;
    font-size: 14px;
    font-weight: 400;
    line-height: 21px;
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

.lateralInfos {
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
    background-color: #000000;
    width: 246px;
    height: 212px;
}
</style>