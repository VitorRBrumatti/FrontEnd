<script>
import Navbar from '@/components/Navbar.vue';
import CriarTarefaModal from '@/components/CriarTarefaModal.vue';
import lateralBar from '@/components/lateralBar.vue';
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
        lateralBar
    },
}
</script>

<template>
    <div class="fundo">
        <div>
            <Navbar @modalEmit="openModal()" />
            <CriarTarefaModal v-model:showModal="showModal" />
        </div>
            <lateralBar></lateralBar>
        <span>Entrada</span>
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


span {
    font-family: Montserrat;
    font-size: 24px;
    font-weight: 800;
    line-height: 29.26px;
    text-align: left;
    color: #000000;
}


</style>