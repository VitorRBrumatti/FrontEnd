<script>
import Navbar from '@/components/Navbar.vue';
import CriarTarefaModal from '@/components/CriarTarefaModal.vue';
import CreateSubTask from '@/components/CreateSubTask.vue';
import lateralBar from '@/components/lateralBar.vue';
import EntradaPage from '@/components/EntradaPage.vue';
import VisualizeTask from '@/components/VisualizeTask.vue';
import axios from 'axios';

export default {
    data() {
        return {
            showModal: false,
            showSubTaskModal: false,
            showVisualize: false,
            selectedTask: {},
            selectedSubTask: {},
            currentFilter: null,
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
        },
        OpenSubTaskModal(subtask) {
            this.selectedSubTask = subtask,
            this.showSubTaskModal = true;
        },
        updateGet() {
            this.$refs.EntradaPageRef.getTasks(null);
        },
        applyFilter(filter) {
            this.currentFilter = filter;
        },
        closeVisu() {
            this.showVisualize = false;
        }
    },
    components: {
        Navbar,
        CriarTarefaModal,
        lateralBar,
        EntradaPage,
        VisualizeTask,
        CreateSubTask

    },
    created () {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api/';
    },
}
</script>

<template>
    <div class="back">
        <lateralBar @filter-tasks="applyFilter"></lateralBar>
        <EntradaPage ref="EntradaPageRef" :filter="currentFilter" @open-edit-modal="OpenEditModal"  @open-visualize-task="OpenVisualizeTask" :update-task-status="updateTaskStatus"></EntradaPage>
        <div>
            <Navbar @modalEmit="openModal()" />
            <CriarTarefaModal @update-get="updateGet" v-model:showModal="showModal" :selectedTask="selectedTask" @get="updateGet"/>
        </div>
        <VisualizeTask @update-get="updateGet" v-model:showVisualize="showVisualize" :selectedTask="selectedTask" @show-Sub-Task="OpenSubTaskModal" ></VisualizeTask>
        <CreateSubTask :selectedSubTask="selectedSubTask" @Visualization="closeVisu" @update-get="updateGet" v-model:showSubTaskModal="showSubTaskModal" :selectedTask="selectedTask" @get="updateGet"></CreateSubTask>
     </div>
</template>

<style scoped>
.back {
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