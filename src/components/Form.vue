<script>
import router from '@/router'
    export default {
        data() {
            return {
                router: this.$route,
            }
        },
        methods: {
            acessHome() {
                this.$router.push('/Home')
            }
        },
    }
</script>

<template>
    <div class="questionary">
        <h1>Login</h1>
        <form>
            <label class="label" for="name">Endereço de e-mail</label>
            <input class="input" type="text" placeholder=" email@mail.com.br">

            <label class="label" for="password">Senha de Acesso</label>
            <input class="input" type="password" placeholder=" *******" id="password">
        </form>
        <div class="funcoes">
            <a href="/Register">Não tenho conta</a>
            <button @click="acessHome()">Acessar</button>
        </div>
    </div>
</template>
<style scoped>
form {
    margin-top: 20px;
    display: grid;
    gap: 14px;
}

.questionary {
    margin-left: 70px;
    margin-top: 100px;
    font-family: Montserrat;
    color: black;
}

.label {
    font-size: 16px;
    font-weight: 400;
    line-height: 28.8px;
    text-align: left;

}

.input {
    font-size: 16px;
    border: 1px;
    width: 535px;
    height: 55px;
    top: 356px;
    left: 684px;
    gap: 0px;
    border: 1px solid #E5E5E5EE;
    border-radius: 0%;
}

.input::placeholder {
    font-size: 16px;
    font-weight: 400;
    line-height: 28.8px;
    text-align: left;

}

h1 {
    width: 81px;
    height: 49px;
    font-weight: 800;
    line-height: 48.6px;
}

button {
    cursor: pointer;
    border: none;
    width: 171px;
    height: 55px;
    font-family: Montserrat;
    font-size: 16px;
    font-weight: 600;
    line-height: 28.8px;
    background-color: #000000;
    color: white;
}

a {
    font-family: Montserrat;
    font-size: 16px;
    font-weight: 400;
    line-height: 28.8px;
    color: #81858E;
}
.funcoes {
    display: flex;
    margin-top: 35px;
    gap: 224px;
    align-items: center;
    
}
</style>