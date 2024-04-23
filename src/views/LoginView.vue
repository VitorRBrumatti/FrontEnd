<script >
    import Login from '../components/Login.vue';


   export default {
    components: {
        Login
    },
   }
</script>

<template>
    <div class="wrapper">
        <div class="container">
            <Login class="quadrado" /> 
            <img src="../assets/beleza.png" alt="Minha imagem" class="beleza">
            <img src="../assets/image.png" alt="Minha imagem"  class="detalhe">
        </div>
        <div class="form">
            <form>
                <h1>Login</h1>
                <label class="label" for="name">Endereço de e-mail</label>
                <input class="input" type="text" placeholder="email@mail.com.br">
    
                <label class="label" for="password">Senha de Acesso</label>
                <input class="input" type="password" placeholder="*******" id="password">
    
            </form>
        </div>
    </div>
</template>

<style scoped>
    .label {
    color: black;
    font-family: Montserrat;
    font-size: 16px;
    font-weight: 400;
    line-height: 28.8px;
    text-align: left;

    }
    .input {
    border: 1px;
    width: 535px;
    height: 55px;
    top: 356px;
    left: 684px;
    gap: 0px;
    border: 1px solid #E5E5E5EE;
    border-radius: 0%;

    }
    .wrapper {
        display: flex;
    }
    .container{
        position: relative;
    }
    .beleza{
    position: absolute;
    width: 48px;
    height: 48px;
    top: 91px;
    left: 96px;
}
    .detalhe{
    position: absolute;
    width: 235px;
    height: 100px;
    top: 459px;
    left: 76px;
    }
    .form {
        margin-left: 70px;
    }
    .quadrado:hover {
    transition: 0.4s;
    filter: invert(1);
    }
    .quadrado:hover + .detalhe:hover{
    transition: 0.4s;
    filter: invert(1);
    }
</style>
