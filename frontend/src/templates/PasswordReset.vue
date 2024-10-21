<template>
    <ToastComponent v-if="showToast" :message="toastMessage" :type="toastType" />
    <div class="container-reset flex justify-center items-center max-w-full pt-1">
        <div id="hea" class="text-center w-full">
            <div class="flex items-center justify-center">
                <img id="logo" src="../assets/img/logo.svg" alt="logo avasoft"
                    class="max-w-[10%] max-h-[10%] opacity-50 mr-4" />
                <div>
                    <h1 class="text-8xl">AVA<span>SOFT</span></h1>
                    <p class="text-center" style="color:#fff">Avaliação Antropometrica</p>
                </div>
            </div>
        </div>
        <span class="mt-4 mb-4 " style="font-size: 25px; color:#fff">Recuperação de senha</span>
        <form action="" id="reset-form" class="w-full max-w-md">
            <div class="flex flex-col">
                <p style="color:#fff">Digite aqui seu email para que possamos enviar a recuperação de senha para seu
                    email.</p>
                <label for="email" id="label-email">Email</label>
                <input type="email" id="email" placeholder="Digite seu email aqui..." v-model="email" class="w-full">
            </div>

            <div class="flex flex-col items-center justify-center gap-3">
                <button type="button" class="button-reset mt-5" @click="handleResetRequest">Enviar</button>
                <a class="mt">
                    <router-link to="/" class="underline decoration-orange-500 text-white hover:text-orange-700">Fazer
                        Login
                    </router-link>
                </a>
            </div>
        </form>
    </div>
</template>

<script>
import ToastComponent from '../components/ToastNotification.vue';

export default {
    components: {
        ToastComponent,
    },
    data() {
        return {
            toastMessage: '',
            toastType: 'success',
            email: '',
            showToast: false,
        };
    },
    methods: {
        handleResetRequest() {
            this.showToast = false; // Reseta a exibição do toast antes de validar

            if (!this.email.trim()) {
                this.showToastMessage('Por favor, insira um e-mail.', 'error');
            } else if (!this.validateEmail(this.email)) {
                this.showToastMessage('Por favor, insira um e-mail válido.', 'error');
            } else {
                // Simulação de envio de e-mail para demonstração
                this.showToastMessage('E-mail enviado com sucesso! Verifique sua caixa de entrada ou spam.', 'success');
            }
        },
        validateEmail(email) {
            const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return emailPattern.test(email);
        },
        showToastMessage(message, type) {
            this.toastMessage = message;
            this.toastType = type;
            this.showToast = true;

            // Define um temporizador para esconder o toast após 3 segundos
            setTimeout(() => {
                this.showToast = false;
            }, 5000);
        },
    },
};
</script>

<style scoped>
p {
    max-width: 100%;
    height: auto;
    margin-bottom: 1rem;
}

#label-email {
    color: #ffffff;
    font-size: 1em;
    margin: 1em 0;
    font-family: "Nanum Gothic", sans-serif;
}

#email {
    width: 100%;
    height: 45px;
    padding: 12px;
    border-radius: 5px;
    border: 1.5px solid lightgrey;
    outline: none;
    transition: all 0.3s cubic-bezier(0.19, 1, 0.22, 1);
    box-shadow: 2px 2px 20px 0px;
}

#email:hover {
    border: 2px solid lightgrey;
    box-shadow: 0px 0px 20px -17px;
}

#email:active {
    transform: scale(0.95);
}

#email:focus {
    border: 2px solid grey;
}

.button-reset {
    background-color: #ff8818;
    color: #ffffff;
    width: 180px;
    height: 50px;
    border-radius: 4px;
    font-size: 18px;
    font-weight: bold;
    cursor: pointer;
    transition: box-shadow .15s, transform .15s;
}

.button-reset:focus {
    box-shadow: #af6a0a 0 0 0 1.5px inset, rgba(45, 35, 66, 0.4) 0 2px 4px, rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #D6D6E7 0 -3px 0 inset;
}

.button-reset:hover {
    box-shadow: rgba(255, 255, 255, 0.278) 0 4px 8px, rgba(45, 35, 66, 0.2) 0 7px 13px -3px, #D6D6E7 0 -3px 0 inset;
    transform: translateY(-2px);
}

.button-reset:active {
    box-shadow: #ffa347 0 3px 7px inset;
    transform: translateY(2px);
}

.container-reset {
    display: flex;
    flex-direction: column;
    height: 100vh;
    width: 100%;
    font-family: "Nanum Gothic", sans-serif;
}
</style>
