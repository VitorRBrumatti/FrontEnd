<script>
export default {
    data() {
        return {
            showing: false
        }
    },
    methods: {
        modalOpen() {
            this.$emit('modalEmit')
        }
    },
}
</script>


<template>
    <nav>
        <img src="../assets/rockLoose.png" alt="RockLoose" class="nav-rock">
        <div class="nav-menu">
            <div class="taskHover" @click="modalOpen()" @mouseover="showing = true" @mouseout="showing = false">
                <img src="../assets/criarTarefa.svg" alt="New task" class="newPlus">
                <p v-show="showing == true" class="taskHover">Criar tarefa</p>
            </div>
            <img src="../assets/ajuda.svg" alt="Help vetor" class="help">
            <img src="../assets/Notification.svg" alt="Notification vetor" class="notification">
            <img src="../assets/Profile.svg" alt="Profile Pic" class="profile-pic">
        </div>
    </nav>
</template>

<style scoped>
nav {
    background-color: black;
    display: flex;
    width: 100%;
    height: 80px;
    position: fixed;
    top: 0%;
    left: 0%;
    margin-inline: auto;
    justify-content: space-between;
    align-items: center;
    padding-inline: 31px;

}

.nav-menu {
    display: flex;
    gap: 63px;
}

.nav-rock {
    width: 33px;
    height: 33px;
}

.profile-pic {
    width: 30px;
    height: 30px;
}

.notification {
    margin-top: 3px;
    width: 25px;
    height: 28px;
}

.newPlus {
    width: 20px;
    height: 20px;
    border: 2px 0px 0px 0px;
}

.taskHover {
    margin-top: 6px;
    display: flex;
    gap: 11px;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}

p {
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 600;
    line-height: 18.29px;
    color: white;
    text-align: center;
}
</style>
