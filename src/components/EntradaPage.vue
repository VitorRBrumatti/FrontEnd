<template>
    <div class="title-container">
        <span id="title-page">Entrada</span>
    </div>
    <div class="tasks-show">
        <div :class="{
            'task-card-subtask': task.subtasks && task.subtasks.length > 0,
            'task-card': !task.subtasks.length > 0
        }" v-for="task in filteredTasks" :key="task.id">
            <div class="task-area">
                <input type="checkbox" :name="'checkbox' + task.id" :id="'custom-checkbox' + task.id"
                    :checked="IsTaskChecked(task)" v-on:change="updateTaskStatus(task)" @click.stop="''" />
                <label :for="'custom-checkbox' + task.id" @click.stop="''"></label>
                <span for="'custom-checkbox' + task.id" @click="visualizeTask(task)">{{ task.title }}</span>
                <p class="description-task">{{ task.description }}</p>
                <div :class="{
                    'today': isToday(task.due_date),
                    'future': !isToday(task.due_date) && !isPastDue(task.due_date),
                    'past-due': isPastDue(task.due_date)
                }" @click.stop="''">
                    <img src="../assets/calendar-valid.svg" v-if="!isPastDue(task.due_date)">
                    <img src="../assets/calendar-expired.svg" v-if="isPastDue(task.due_date)">
                    <p>{{ isToday(task.due_date) ? "Hoje" : formatDueDate(task.due_date) }}</p>
                </div>
                <div class="line" v-if="task.subtasks && task.subtasks.length > 0">
                </div>
                <div class="subtask-area">
                    <div v-if="task.subtasks && task.subtasks.length > 0">
                        <div class="subtask-organize">
                            <div class="show-subtask" v-for="Subtask in task.subtasks" :key="Subtask.id"
                                @click.stop="''">
                                <input type="checkbox" :name="'checkbox' + Subtask.id"
                                    :id="'custom-checkbox' + Subtask.id" v-model="IsTaskChecked"
                                    @change="updateTaskStatus" />
                                <label :for="'custom-checkbox' + Subtask.id"></label>
                                <span @click.stop>{{ Subtask.title_subtask }}</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="hover-icons" :class="{
                'hover-icons-subtask': task.subtasks && task.subtasks.length > 0,
                'hover-icons': !task.subtasks.length > 0
            }">
                <div class="hovered-edit">
                    <span id="edit-tooltip">Editar tarefa</span>
                    <img src="../assets/edit-icon.svg" alt="edit-icon" @click="openEditModal(task)" @click.stop="''">
                </div>
                <div class="hovered-due-date">
                    <span id="date-tooltip">Definir vencimento</span>
                    <img src="../assets/set-due-date.svg" alt="set-due-date" @click.stop="''">
                </div>
                <div class="hovered-delete">
                    <img src="../assets/delete-icon.svg" alt="delete-icon"
                        @click="selectedTasks = task.id, deleteTask(task.id)" @click.stop="''">
                    <span id="delete-tooltip">Excluir tarefa</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    props: {
        filter: String,
    },
    data() {
        return {
            selectedTasks: {},
            Tasks: [],
            filteredTasks: [],
            taskIsChecked: [],
        };
    },
    watch: {
        filter: {
            immediate: true,
            handler(newFilter) {
                this.getTasks(newFilter);
            }
        }
    },
    methods: {
        getTasks(filter = null) {
            console.log('getTask is working');
            axios.get('/task', { params: { filter } })
                .then((response) => {
                    console.log(response);
                    this.Tasks = response.data.data;
                    this.taskIsChecked = this.Tasks.map(task => ({ id: task.id, status: task.status }));
                    this.filteredTasks = this.Tasks;
                    console.log(this.Tasks);
                })
                .catch(error => {
                    alert('Failed to fetch tasks:', error);
                });
        },
        IsTaskChecked(task) {
            return this.taskIsChecked.some(item => item.id === task.id && item.status === 'completed');
        },
        updateTaskStatus(task) {
            let newStatus = task.status === 'completed' ? 'pending' : 'completed';

            axios.put(`/task/${task.id}`, { status: newStatus })
                .then(response => {
                    console.log(`Task updated successfully: ${response.data}`);

                    let taskToUpdate = this.taskIsChecked.find(item => item.id === task.id);

                    if (taskToUpdate) {
                        taskToUpdate.status = newStatus;
                    } else {
                        this.taskIsChecked.push({ id: task.id, status: newStatus });
                    }

                    task.status = newStatus;
                })
                .catch(error => {
                    console.error(`Error updating task: ${error}`);
                });
        },
        openEditModal(task) {
            this.$emit('open-edit-modal', task);
        },
        formatDueDate(dueDate) {
            const dateObject = new Date(dueDate);
            const formattedDate = dateObject.toISOString().split('T')[0];
            return formattedDate;
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
        deleteTask(task) {
            axios.delete(`/task/${task}`)
                .then(() => {
                    let deletedTask = this.Tasks.findIndex(item => item.id === task);
                    this.Tasks.splice(deletedTask, 1);
                    this.$emit('update:showVisualize', false);
                })
                .catch(error => {
                    console.error(`Error deleting task: ${error}`);
                });
        },
        visualizeTask(task) {
            this.$emit('open-visualize-task', task);
        },
    },
    created() {
        this.getTasks();
    },
};

</script>

<style scoped>
* {
    font-family: Montserrat;
}

.title-container {
    position: absolute;
    width: 701px;
    height: 109px;
    align-self: self-start;
    margin-top: 80px;
    margin-left: 1.5%;
    display: flex;
    align-items: flex-end;
}

#title-page {
    font-size: 24px;
    font-weight: 800;
    line-height: 29.26px;
    text-align: left;
    color: #000000;
}

.tasks-show {
    display: flex;
    flex-direction: column;
    gap: 15px;
    align-self: self-start;
    margin-top: 14%;
    height: 65vh;
    overflow-y: auto;
    overflow-x: hidden;
    position: absolute;
}

.content {
    display: flex;
}

.content-tasks {
    display: block;
}

.hover-icons {
    display: flex;
    position: relative;
    gap: 25px;
    opacity: 0;
    transition: 0.3s opacity ease;
}

.hover-icons img {
    display: flex;
    cursor: pointer;
}

.hover-icons-subtask {
    display: flex;
    position: relative;
    gap: 25px;
    right: 60%;
    bottom: 20%;
    opacity: 1;
    transition: 0.3s opacity ease;
}

.hover-icons-subtask img {
    display: flex;
    cursor: pointer;
}

.hovered-edit:hover #edit-tooltip,
.hovered-due-date:hover #date-tooltip,
.hovered-delete:hover #delete-tooltip {
    opacity: 1;
    visibility: visible;
    transition-delay: 0s;

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
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s ease, visibility 0s linear 0.3s;

}

.task-card {
    border: 1px solid #E5E5E5;
    padding: 15px 22px;
    width: 678px;
    height: 109px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
}

.task-card:hover {
    background-color: #FAFAFA;
}

.task-card:hover .hover-icons {
    opacity: 1;
}

.task-card input[type="checkbox"] {
    display: none;

}

.task-card span {
    color: #000000;
    font-weight: 500;
}

.task-card label:before {
    content: '';
    width: 18px;
    height: 18px;
    border-radius: 50%;
    border: 1px solid rgb(0, 0, 0);
    display: inline-block;
    vertical-align: middle;
    margin-right: 30px;
    margin-bottom: 3px;
    transition: background-color 0.3s ease;
}

.task-card input[type="checkbox"]:checked+label::before {
    background-color: rgb(0, 0, 0);
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='18' height='18' viewBox='0 0 10 10'%3E%3Cg class='nc-icon-wrapper' stroke-width='1' fill='%23555555'%3E%3Cpath fill='none' stroke='%23FFFFFF' stroke-linecap='round' stroke-linejoin='round' stroke-miterlimit='10' data-cap='butt' d='M2.83 4.72l1.58 1.58 2.83-2.83'/%3E%3C/g%3E%3C/svg%3E");
    background-position: center;
    border: none;
    padding: 1px;
    transition: background-color 0.3s ease;
}

.description-task {
    color: #81858E;
    margin-left: 48px;
}


.today {
    background: #0094881A;
    color: #00948800 !important;
    font-weight: 500;
    width: 72px;
    height: 25px;
    margin-left: 48px;
    margin-top: 11px;
    display: flex;
    cursor: default;

}

.today p {
    color: #009488;
    position: relative;
    left: 12px;
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    top: 4px;
}

.today img {
    width: 13;
    height: 14;
    top: -3px;
    left: 5px;
    margin-top: 6px;
    position: relative;

}


.past-due {
    background: #D314081A;
    color: #D31408 !important;
    font-weight: 500;
    width: 130px;
    height: 30px;
    position: relative;
    left: 47px;
    display: flex;
    top: 5px;
    cursor: default;
}

.past-due img {
    width: 13;
    height: 14;
    top: -6px;
    left: 5px;
    margin-top: 12px;
    position: relative;
}

.past-due p {
    color: #D31408;
    position: relative;
    left: 16px;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    top: 4px;
}

.future {
    background: #0094881A;
    color: #009488 !important;
    font-weight: 500;
    width: 130px;
    height: 30px;
    position: relative;
    left: 49px;
    top: 5px;
    display: flex;
    cursor: default;
}

.future img {
    width: 13;
    height: 14;
    top: -6px;
    left: 5px;
    margin-top: 12px;
    position: relative;

}

.future p {
    color: #009488;
    position: relative;
    left: 16px;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    top: 4px;
}

.subtask-organize {
    display: flex;
    flex-direction: column;
    position: relative;
}

.task-card-subtask {
    border: 1px solid #E5E5E5;
    padding: 15px 22px;
    width: 678px;
    height: 200px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
}

.line {
    border: 1px solid #E5E5E5;
    position: relative;
    margin-top: 20px;
    margin-bottom: 20px;
    width: 100vh;
    margin-left: -50px;
}

.task-card-subtask:hover {
    background-color: #FAFAFA;
}

.task-card-subtask:hover .hover-icons {
    opacity: 1;
}

.task-card-subtask input[type="checkbox"] {
    display: none;

}

.task-card-subtask span {
    color: #000000;
    font-weight: 500;
}

.task-card-subtask label::before {
    content: '';
    width: 18px;
    height: 18px;
    border-radius: 50%;
    border: 1px solid rgb(0, 0, 0);
    display: inline-block;
    vertical-align: middle;
    margin-right: 30px;
    margin-bottom: 3px;
    transition: background-color 0.3s ease;
}

.task-card-subtask input[type="checkbox"]:checked+label::before {
    background-color: rgb(0, 0, 0);
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='18' height='18' viewBox='0 0 10 10'%3E%3Cg class='nc-icon-wrapper' stroke-width='1' fill='%23555555'%3E%3Cpath fill='none' stroke='%23FFFFFF' stroke-linecap='round' stroke-linejoin='round' stroke-miterlimit='10' data-cap='butt' d='M2.83 4.72l1.58 1.58 2.83-2.83'/%3E%3C/g%3E%3C/svg%3E");
    background-position: center;
    border: none;
    padding: 1px;
    transition: background-color 0.3s ease;
}
</style>