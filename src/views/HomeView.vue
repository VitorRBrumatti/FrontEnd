<script>
import Navbar from '@/components/Navbar.vue';
import CriarTarefaModal from '@/components/CriarTarefaModal.vue';
import lateralBar from '@/components/lateralBar.vue';
import EntradaPage from '@/components/EntradaPage.vue';
import ExpiratedPage from '@/components/ExpiratedPage.vue';
import TaskToday from '@/components/TaskToday.vue';
import axios from 'axios';

export default {
    data() {
        return {
            showModal: false,
        }
    },
    methods: {
        openModal() {
            this.showModal = true;
        }
    },
    components: {
        Navbar,
        CriarTarefaModal,
        lateralBar,
        EntradaPage,
        ExpiratedPage,
        TaskToday

    },
    created () {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api/';
    },
}
</script>

<template>
    <div class="fundo">
        <lateralBar></lateralBar>
        <EntradaPage></EntradaPage>
        <div>
            <Navbar @modalEmit="openModal()" />
            <CriarTarefaModal v-model:showModal="showModal" />
        </div>
     </div>

</template>

<style scoped>
.fundo {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
}


</style>