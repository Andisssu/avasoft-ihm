<template>
  <ToastComponent v-if="showToast" :message="toastMessage" :type="toastType" />
  <div class="container-login pt-28 flex flex-col items-start md:items-start md:flex-row justify-between mx-auto p-6">
    <link rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Dela+Gothic+One&family=Kode+Mono:wght@400..700&family=Nanum+Gothic&display=swap">
    
    <!-- Header -->
    <header class="flex flex-col pt-20 justify-center items-start md:items-start text-center md:text-left xl:m-0 md:pl-14">
      <h1 class="text-5xl px-48 lg:text-8xl mt-3 text-white">AVA<span class="text-orange-400">SOFT</span></h1>
      <p class="text-white px-64 text-center md:text-left">Avaliação Antropométrica</p>
    </header>

    <!-- Formulário de Login -->
    <form id="login-form" class="w-full max-w-md md:ml-16 mt-8 md:mt-0">
      <div class="flex flex-col space-y-4">
        <label for="name" id="label-name" class="text-white">Nome</label>
        <input type="text" id="name" v-model="userName"
          class="p-3 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-400 transition-all" />

        <label for="pass" id="label-pass" class="text-white">Senha</label>
        <input type="password" id="pass" v-model="password"
          class="p-3 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-400 transition-all" />
      </div>

      <p class="text-right text-white pt-2">
        <a class="text-orange-400 hover:underline"><router-link to="/PasswordReset">Esqueceu a senha?</router-link></a>
      </p>

      <div class="flex justify-center mt-4">
        <button type="button" class="button-login" @click="handleLogin">Entrar</button>
      </div>

      <div class="text-center text-white pt-4">
        <p>Não tem uma conta? 
          <a class="text-orange-400 hover:underline"><router-link to="/ProfessionalRegister">Registre-se aqui!</router-link></a>
        </p>
        <p class="pt-4">__________ OU __________</p>

        <div id="micro-google" class="flex justify-around pt-4 space-x-4">
          <GoogleLogin :callback="callback" prompt auto-login />
          <a href="#" class="flex items-center justify-center space-x-2 bg-gray-700 rounded-md px-4 py-2">
            <img src="../assets/img/microsoft.png" alt="Microsoft" class="w-6 h-auto" />
            <span>Microsoft</span>
          </a>
        </div>
      </div>
    </form>
  </div>

  <!-- Logo de fundo -->
  <img id="logo" src="../assets/img/logo.svg" alt="logo avasoft"
    class="fixed top-1/2 right-0 transform -translate-y-1/2 opacity-50 z-[-1] max-w-full max-h-full" />
</template>

<script>
import { googleTokenLogin } from "vue3-google-login"
import { decodeCredential } from "vue3-google-login";
import ToastComponent from '../components/ToastNotification.vue';

export default {
  components: {
    ToastComponent
  },

  data() {
    return {
      userName: '',
      password: '',
      showToast: false,
      toastMessage: '',
      toastType: 'success',
      users: [
        { userName: 'user1', password: 'password1' },
        { userName: 'user2', password: 'password2' }
      ]
    }
  },

  mounted() {
    const urlParams = new URLSearchParams(window.location.search);
    const token = urlParams.get('token');

    if (token) {
      localStorage.setItem('authToken', token);
      this.$axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
      this.$router.push('/HomePage');
    } else {
      const authToken = localStorage.getItem('authToken');
      if (authToken) {
        this.$axios.defaults.headers.common['Authorization'] = `Bearer ${authToken}`;
      }
    }

    document.documentElement.style.overflow = 'hidden';
  },
  beforeUnmount() {
    document.documentElement.style.overflow = '';
  },

  methods: {
    callback(response) {
      const userData = decodeCredential(response.credential)
      console.log("Handle the userData", userData)
    },
    logingoogle() {
      googleTokenLogin().then((response) => {
        console.log("Handle the response", response)
      })
    },
    handleLogin() {
      const user = this.users.find(user => user.userName === this.userName && user.password === this.password);
      if (user) {
        this.showToastMessage('Login efetuado com sucesso!', 'success');
        this.$router.push('/HomePage');
      } else {
        this.showToastMessage('Erro ao fazer login! Revise sua senha e nome de usuario', 'error');
      }
    },
    showToastMessage(message, type) {
      this.toastMessage = message;
      this.toastType = type;
      this.showToast = false;
      this.$nextTick(() => {
        this.showToast = true;
      });
    },
  },
};
</script>

<style scoped>
/* Estilos Personalizados */
#label-name,
#label-pass {
  font-size: 1em;
  margin: 0.5em 0;
  font-family: "Nanum Gothic", sans-serif;
}

.button-login {
  margin-top: 1em;
  background-color: #ff8818;
  border-radius: 5px;
  color: white;
  font-size: 18px;
  font-weight: bold;
  padding: 0.5em 1em;
  transition: transform 0.15s, box-shadow 0.15s;
}

.button-login:hover {
  box-shadow: rgba(255, 255, 255, 0.278) 0 4px 8px, rgba(45, 35, 66, 0.2) 0 7px 13px -3px;
  transform: translateY(-1px);
}

.button-login:active {
  box-shadow: inset 0 3px 7px #ffa347;
  transform: translateY(0.8px);
}
</style>