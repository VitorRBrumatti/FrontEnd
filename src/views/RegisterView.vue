<script>
import Login from '../components/Login.vue';
import FormRegister from '@/components/FormRegister.vue';
  export default {
    components: {
      Login,
      FormRegister
    },
    }
</script>

<template>
    <div class="fundo">
      <FormRegister></FormRegister>
      <Login class="square"></Login>
    </div>
</template>


<style scoped>
.fundo {
  display: flex;
}
.square {
  margin-left: 60px;
}
</style>