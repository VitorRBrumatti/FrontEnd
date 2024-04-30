<template>
    <div class="title-container">
        <span id="title-page">Entrada</span>
    </div>
    <div class="Tasks-show" v-for="task in Tasks" :key="task.id">
        <input type="checkbox" name="checkbox" id="custom-checkbox">
        <label for="custom-checkbox">{{ task.title }}</label>
        <p class="description-task">{{ task.description }}</p>
        <input class="date" type="date">
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            Tasks: [],
        }
    },
    methods: {
        getTasks() {
            console.log('getTask is working');
            axios.get('/task/0')
                .then((response) => {
                    console.log(response);
                    this.Tasks = response.data.data
                    console.log(this.Tasks);
                });
        }
    },
    created () {
        this.getTasks();
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
    margin-left: 5%;
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
    margin-top: 20px;
    border: 1px solid #E5E5E5;
    padding: 15px 22px;
    gap: 15px;
}

.Tasks-show input[type="checkbox"] {
    display: none;
}

.Tasks-show label {
    color: #000000;
    font-weight: 500;
}

.Tasks-show label:before {
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

.Tasks-show input[type="checkbox"]:checked+label:before {
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
input.date:hover{
    color: #009488;
    background: #0094881A;
}
</style>