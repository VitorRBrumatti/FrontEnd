<script>
import Navbar from '@/components/Navbar.vue';
import CriarTarefaModal from '@/components/CriarTarefaModal.vue';
import lateralBar from '@/components/lateralBar.vue';
import EntradaPage from '@/components/EntradaPage.vue';
import VisualizeTask from '@/components/VisualizeTask.vue';
import axios from 'axios';

export default {
    data() {
        return {
            showModal: false,
            showVisualize: false,
            selectedTask: {},
        }
    },
    methods: {
        openModal() {
            this.selectedTask = {},
            this.showModal = true;
        },
        OpenEditModal(task) {
            this.selectedTask = task,
            this.showModal = true;
        },
        OpenVisualizeTask(task) {
            this.selectedTask = task,
            this.showVisualize = true;
        }
    },
    components: {
        Navbar,
        CriarTarefaModal,
        lateralBar,
        EntradaPage,
        VisualizeTask,

    },
    created () {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api/';
    },
}
</script>

<template>
    <div class="fundo">
        <lateralBar></lateralBar>
        <EntradaPage @open-edit-modal="OpenEditModal"  @open-visualize-task="OpenVisualizeTask" :update-task-status="updateTaskStatus"></EntradaPage>
        <div>
            <Navbar @modalEmit="openModal()" />
            <CriarTarefaModal v-model:showModal="showModal" :selectedTask="selectedTask"/>
        </div>
        <VisualizeTask v-model:showVisualize="showVisualize" :selectedTask="selectedTask" @checkbox-changed="updateTaskStatus"></VisualizeTask>
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