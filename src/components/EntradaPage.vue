<template>
    <div class="title-container">
        <span id="title-page">Entrada</span>
    </div>
    <div class="Tasks-show">
        <div class="task-card" v-for="task in Tasks" :key="task.id">

            <div>
                <input type="checkbox" :name="'checkbox' + task.id" :id="'custom-checkbox' + task.id"
                    :checked="IsTaskChecked(task)" v-on:change="updateTaskStatus(task)" />
                <label :for="'custom-checkbox' + task.id"></label>
                <span for="'custom-checkbox' + task.id">{{ task.title }}</span>
                <p class="description-task">{{ task.description }}</p>
                <input class="date" type="date">
            </div>
            <div class="hover-icons">
                <div class="hovered-edit">
                    <span id="edit-tooltip">Editar tarefa</span>
                    <img src="../assets/edit-icon.svg" alt="edit-icon">
                </div>
                <div class="hovered-due-date">
                    <span id="date-tooltip">Definir vencimento</span>
                    <img src="../assets/set-due-date.svg" alt="set-due-date">
                </div>
                <div class="hovered-delete">
                    <img src="../assets/delete-icon.svg" alt="delete-icon">
                    <span id="delete-tooltip">Excluir tarefa</span>
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
            selectedTasks: {},
            Tasks: [],
            taskIsChecked: [],
        }
    },
    methods: {
        getTasks() {
            console.log('getTask is working');
            axios.get('/task')
                .then((response) => {
                    console.log(response);
                    this.Tasks = response.data.data;
                    this.taskIsChecked = this.Tasks.map(task => ({ id: task.id, status: task.status }));
                    console.log(this.Tasks);
                });
        },
        IsTaskChecked(task) {
            return this.taskIsChecked.some(item => item.id === task.id && item.status === 'completed');
        },
        updateTaskStatus(task) {
            const newStatus = task.status === 'completed' ? 'pending' : 'completed';

            axios.put(`/task/${task.id}`, { status: newStatus })
                .then(response => {
                    console.log(`Task updated successfully: ${response.data}`);

                    const taskToUpdate = this.taskIsChecked.find(item => item.id === task.id);

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
        }
    },
    created() {
        this.getTasks();
    },
    watch: {
        data(newValue, oldValue) {

        }
    },
}
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

.Tasks-show {
    display: flex;
    flex-direction: column;
    gap: 15px;
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

.task-card input[type="checkbox"]:checked+label:before {
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

.date {
    width: 199px;
    height: 40px;
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 400;
    line-height: 17.07px;
    border: 1px solid #E5E5E5;
    margin-left: 48px;
}

input.date {
    font-family: Montserrat;
    color: #E5E5E5;
    padding-left: 12%;
    cursor: pointer;
}

input.date::-webkit-calendar-picker-indicator {
    position: relative;
    right: 115%;
    background-size: 17px;
    color: #E5E5E5;
    background-position: center;
    background-image: url('../assets/calendar.svg');
}

input.date:hover {
    color: #009488;
    background: #0094881A;
}
</style>