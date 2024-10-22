<template>
  <ToastComponent v-if="showToast" :message="toastMessage" :type="toastType" />
  <div class="container flex flex-col justify-start items-center min-h-screen text-white px-4">
    <h2 class="text-2xl mb-6">Cadastro de Profissionais</h2>

    <form @submit.prevent="handleRegister" id="register-form" class="w-full max-w-4xl text-black">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-2">
        <div class="flex flex-col mx-4">
          <label for="fullName" class="mb-2 text-white">Nome completo <span class="text-red-500">*</span></label>
          <input type="text" id="fullName" placeholder="Digite seu nome completo aqui..." v-model="fullName"
            class="w-full p-3 rounded focus:border-orange-500" />
          <span v-if="errors.fullName" class="text-red-500 text-sm">{{ errors.fullName }}</span>
        </div>

        <div class="flex flex-col mx-4">
          <label for="cpf" class="mb-2 text-white">CPF <span class="text-red-500">*</span></label>
          <input type="text" id="cpf" placeholder="Digite seu CPF aqui..." v-model="cpf"
            class="w-full p-3 rounded focus:border-orange-500" />
          <span v-if="errors.cpf" class="text-red-500 text-sm">{{ errors.cpf }}</span>
        </div>

        <div class="flex flex-col mx-4">
          <label for="phone" class="mb-2 text-white">Telefone <span class="text-red-500">*</span></label>
          <input type="text" id="phone" placeholder="Digite seu telefone aqui..." v-model="phone"
            class="w-full p-3 rounded focus:border-orange-500" />
          <span v-if="errors.phone" class="text-red-500 text-sm">{{ errors.phone }}</span>
        </div>

        <div class="flex flex-col mx-4">
          <label for="email" class="mb-2 text-white">Email <span class="text-red-500">*</span></label>
          <input type="email" id="email" placeholder="Digite seu email aqui..." v-model="email"
            class="w-full p-3 rounded focus:border-orange-500" />
          <span v-if="errors.email" class="text-red-500 text-sm">{{ errors.email }}</span>
        </div>

        <div class="flex flex-col mx-4">
          <label for="userName" class="mb-2 text-white">Nome de Usuário <span class="text-red-500">*</span></label>
          <input type="text" id="userName" placeholder="Digite seu nome de usuário aqui..." v-model="userName"
            class="w-full p-3 rounded focus:border-orange-500" />
          <span v-if="errors.userName" class="text-red-500 text-sm">{{ errors.userName }}</span>
        </div>

        <div class="flex flex-col mx-4">
          <label for="password" class="mb-2 text-white">Senha <span class="text-red-500">*</span></label>
          <input type="password" id="password" placeholder="Digite sua senha aqui..." v-model="password"
            class="w-full p-3 rounded focus:border-orange-500" />
          <span v-if="errors.password" class="text-red-500 text-sm">{{ errors.password }}</span>
        </div>

        <div class="flex flex-col mx-4">
          <label for="confirmPassword" class="mb-2 text-white">Confirmação de Senha <span class="text-red-500">*</span></label>
          <input type="password" id="confirmPassword" placeholder="Confirme sua senha aqui..." v-model="confirmPassword"
            class="w-full p-3 rounded focus:border-orange-500" />
          <span v-if="errors.confirmPassword" class="text-red-500 text-sm">{{ errors.confirmPassword }}</span>
        </div>

        <div class="flex flex-col mx-4">
          <label for="street" class="mb-2 text-white">Rua</label>
          <input type="text" id="street" placeholder="Digite sua rua aqui..." v-model="street"
            class="w-full p-3 rounded focus:border-orange-500" />
            <span v-if="errors.street" class="text-red-500 text-sm">{{ errors.street }}</span>

        </div>

        <div class="flex flex-col mx-4">
          <label for="number" class="mb-2 text-white">Número <span class="text-red-500">*</span></label>
          <input type="number" id="number" placeholder="Digite o número aqui..." v-model="number"
            class="w-full p-3 rounded focus:border-orange-500" />
            <span v-if="errors.number" class="text-red-500 text-sm">{{ errors.number }}</span>

        </div>

        <div class="flex flex-col mx-4">
          <label for="complement" class="mb-2 text-white">Complemento</label>
          <input type="text" id="complement" placeholder="Digite o complemento aqui..." v-model="complement"
            class="w-full p-3 rounded focus:border-orange-500" />
            
        </div>

        <div class="flex flex-col mx-4">
          <label for="district" class="mb-2 text-white">Bairro <span class="text-red-500">*</span></label>
          <input type="text" id="district" placeholder="Digite seu bairro aqui..." v-model="district"
            class="w-full p-3 rounded focus:border-orange-500" />
            <span v-if="errors.district" class="text-red-500 text-sm">{{ errors.district }}</span>

        </div>

        <div class="flex flex-col mx-4">
          <label for="city" class="mb-2 text-white">Cidade <span class="text-red-500">*</span></label>
          <input type="text" id="city" placeholder="Digite sua cidade aqui..." v-model="city"
            class="w-full p-3 rounded focus:border-orange-500" />
            <span v-if="errors.city" class="text-red-500 text-sm">{{ errors.city }}</span>

        </div>

        <div class="flex flex-col mx-4">
          <label for="state" class="mb-2 text-white">Estado <span class="text-red-500">*</span></label>
          <input type="text" id="state" placeholder="Digite seu estado aqui..." v-model="state"
            class="w-full p-3 rounded focus:border-orange-500" />
            <span v-if="errors.state" class="text-red-500 text-sm">{{ errors.state }}</span>

        </div>

        <div class="flex flex-col mx-4">
          <label for="cep" class="mb-2 text-white">CEP <span class="text-red-500">*</span></label>
          <input type="text" id="cep" placeholder="Digite seu CEP aqui..." v-model="cep"
            class="w-full p-3 rounded focus:border-orange-500" />
          <span v-if="errors.cep" class="text-red-500 text-sm">{{ errors.cep }}</span>
        </div>
      </div>

      <div class="flex justify-center mt-6 gap-4">
        <button type="button" @click="goBack" class="bg-gray-500 text-white p-3 rounded">Voltar</button>
        <button type="submit" class="bg-orange-500 text-white p-3 rounded">Cadastrar</button>
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
      fullName: '',
      cpf: '',
      phone: '',
      email: '',
      userName: '',
      password: '',
      confirmPassword: '',
      street: '',
      number: '',
      complement: '',
      district: '',
      city: '',
      state: '',
      cep: '',
      errors: {},
      toastMessage: '',
      toastType: '',
      showToast: false,
    };
  },
  methods: {
    validateCPF(cpf) {
      // Lógica de validação de CPF (apenas exemplo)
      return cpf.length === 11 && !isNaN(cpf);
    },
    validatePhone(phone) {
      // Lógica de validação de telefone (apenas exemplo)
      return phone.length >= 10 && phone.length <= 15 && !isNaN(phone);
    },
    validateCEP(cep) {
      // Lógica de validação de CEP (apenas exemplo)
      return cep.length === 8 && !isNaN(cep);
    },
    handleRegister() {
  this.errors = {}; // Limpa os erros

  // Validação dos campos
  if (!this.fullName) this.errors.fullName = 'Nome completo é obrigatório.';
  if (!this.cpf || !this.validateCPF(this.cpf)) this.errors.cpf = 'CPF inválido. Apenas números';
  if (!this.phone || !this.validatePhone(this.phone)) this.errors.phone = 'Telefone inválido.';
  if (!this.email) this.errors.email = 'Email é obrigatório.';
  if (!this.userName) this.errors.userName = 'Nome de usuário é obrigatório.';
  if (!this.password) this.errors.password = 'Senha é obrigatória.';
  if (!this.city) this.errors.city = 'Cidade é obrigatória.';
  if (!this.state) this.errors.state = 'Estado é obrigatório.';
  if (!this.district) this.errors.district = 'Bairro é obrigatório.';
  if (!this.number) this.errors.number = 'Número é obrigatória.';
  if (!this.street) this.errors.street = 'Rua é obrigatória.';
  if (this.password !== this.confirmPassword) this.errors.confirmPassword = 'As senhas não coincidem.';
  if (!this.cep || !this.validateCEP(this.cep)) this.errors.cep = 'CEP inválido, digite apenas números.';

  // Verifica se há erros
  if (Object.keys(this.errors).length > 0) {
    this.toastMessage = 'Por favor, corrija os erros no formulário.';
    this.toastType = 'error';
    this.showToast = false; // Garante que o toast é fechado antes
    this.$nextTick(() => {
      this.showToast = true; // Exibe o toast novamente
    });
    return;
  }

  // Após o sucesso, exiba uma mensagem de sucesso
  this.toastMessage = 'Profissional cadastrado com sucesso!';
  this.toastType = 'success';
  this.showToast = false; // Garante que o toast é fechado antes
  this.$nextTick(() => {
    this.showToast = true; // Exibe o toast novamente
  });

  // Limpa os campos
  this.clearFields();
},

    clearFields() {
      this.fullName = '';
      this.cpf = '';
      this.phone = '';
      this.email = '';
      this.userName = '';
      this.password = '';
      this.confirmPassword = '';
      this.street = '';
      this.number = '';
      this.complement = '';
      this.district = '';
      this.city = '';
      this.state = '';
      this.cep = '';
      this.errors = {};
    },
    goBack() {
      this.$router.go(-1);
    },
  },
};
</script>

<style scoped>
/* Estilos personalizados */
</style>
