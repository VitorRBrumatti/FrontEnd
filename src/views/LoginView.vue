<script>
import Login from '../components/Login.vue';
import FormLogin from '../components/Form.vue'


export default {
    components: {
        Login,
        FormLogin
    },
}
</script>

<template>
    <div class="wrapper">
        <Login />
        <FormLogin></FormLogin>
    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

.wrapper {
    display: flex;
}
</style>
