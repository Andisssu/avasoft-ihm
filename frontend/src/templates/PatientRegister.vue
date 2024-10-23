<template>
  <ToastComponent v-if="showToast" :message="toastMessage" :type="toastType" />
  <div class="container flex flex-col justify-start items-center min-h-screen text-white px-4">
    <!-- Título do formulário -->
    <h2 class="text-2xl mb-6">Cadastro de Pacientes</h2>

    <!-- Formulário de cadastro -->
    <form @submit.prevent="handleRegister" id="register-form" class="w-full max-w-4xl text-black">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-2">
        <!-- Nome completo -->
        <div class="flex flex-col mx-4">
          <label for="fullName" class="mb-2 text-white">Nome completo <span class="text-red-500">*</span></label>
          <input type="text" id="fullName" placeholder="Digite seu nome completo aqui..." v-model="fullName"
            class="w-full p-3 rounded focus:border-orange-500" required>
        </div>

        <!-- CPF -->
        <div class="flex flex-col mx-4">
          <label for="cpf" class="mb-2 text-white">CPF <span class="text-red-500">*</span></label>
          <input type="text" id="cpf" placeholder="Digite seu CPF aqui..." v-model="cpf"
            class="w-full p-3 rounded focus:border-orange-500" required>
          <span v-if="cpfError" class="text-red-500 text-sm">{{ cpfError }}</span>
        </div>

        <!-- Data de Nascimento -->
        <div class="flex flex-col mx-4">
          <label for="dataNasc" class="mb-2 text-white">Data de Nascimento <span class="text-red-500">*</span></label>
          <input type="date" id="dataNasc" v-model="dataNasc" class="w-full p-3 rounded focus:border-orange-500" required>
        </div>

        <!-- Gênero -->
        <div class="flex flex-col mx-4">
          <label for="gender" class="mb-2 text-white">Gênero <span class="text-red-500">*</span></label>
          <select id="gender" v-model="gender" class="w-full p-3 rounded focus:border-orange-500" required>
            <option value="" disabled selected>Selecione seu gênero</option>
            <option value="Masculino">Masculino</option>
            <option value="Feminino">Feminino</option>
            <option value="Outro">Outro</option>
          </select>
        </div>

        <!-- Telefone -->
        <div class="flex flex-col mx-4">
          <label for="phone" class="mb-2 text-white">Telefone <span class="text-red-500">*</span></label>
          <input type="text" id="phone" placeholder="Digite seu telefone aqui..." v-model="phone"
            class="w-full p-3 rounded focus:border-orange-500" required>
          <span v-if="phoneError" class="text-red-500 text-sm">{{ phoneError }}</span>
        </div>

        <!-- Email -->
        <div class="flex flex-col mx-4">
          <label for="email" class="mb-2 text-white">Email <span class="text-red-500">*</span></label>
          <input type="email" id="email" placeholder="Digite seu email aqui..." v-model="email"
            class="w-full p-3 rounded focus:border-orange-500" required>
          <span v-if="emailError" class="text-red-500 text-sm">{{ emailError }}</span>
        </div>

        <!-- Nome de Usuário -->
        <div class="flex flex-col mx-4">
          <label for="userName" class="mb-2 text-white">Nome de Usuário <span class="text-red-500">*</span></label>
          <input type="text" id="userName" placeholder="Digite seu nome de usuário aqui..." v-model="userName"
            class="w-full p-3 rounded focus:border-orange-500" required>
        </div>

        <!-- Rua -->
        <div class="flex flex-col mx-4">
          <label for="street" class="mb-2 text-white">Rua <span class="text-red-500">*</span></label>
          <input type="text" id="street" placeholder="Digite sua rua aqui..." v-model="street"
            class="w-full p-3 rounded focus:border-orange-500" required>
        </div>

        <!-- Número -->
        <div class="flex flex-col mx-4">
          <label for="number" class="mb-2 text-white">Número <span class="text-red-500">*</span></label>
          <input type="number" id="number" placeholder="Digite o número aqui..." v-model="number"
            class="w-full p-3 rounded focus:border-orange-500" required>
        </div>

        <!-- Complemento -->
        <div class="flex flex-col mx-4">
          <label for="complement" class="mb-2 text-white">Complemento</label>
          <input type="text" id="complement" placeholder="Digite o complemento aqui..." v-model="complement"
            class="w-full p-3 rounded focus:border-orange-500">
        </div>

        <!-- Bairro -->
        <div class="flex flex-col mx-4">
          <label for="district" class="mb-2 text-white">Bairro <span class="text-red-500">*</span></label>
          <input type="text" id="district" placeholder="Digite seu bairro aqui..." v-model="district"
            class="w-full p-3 rounded focus:border-orange-500" required>
        </div>

        <!-- Cidade -->
        <div class="flex flex-col mx-4">
          <label for="city" class="mb-2 text-white">Cidade <span class="text-red-500">*</span></label>
          <input type="text" id="city" placeholder="Digite sua cidade aqui..." v-model="city"
            class="w-full p-3 rounded focus:border-orange-500" required>
        </div>

        <!-- Estado -->
        <div class="flex flex-col mx-4">
          <label for="state" class="mb-2 text-white">Estado <span class="text-red-500">*</span></label>
          <input type="text" id="state" placeholder="Digite seu estado aqui..." v-model="state"
            class="w-full p-3 rounded focus:border-orange-500" required>
        </div>

        <!-- CEP -->
        <div class="flex flex-col mx-4">
          <label for="cep" class="mb-2 text-white">CEP <span class="text-red-500">*</span></label>
          <input type="text" id="cep" placeholder="Digite seu CEP aqui..." v-model="cep"
            class="w-full p-3 rounded focus:border-orange-500" required>
          <span v-if="cepError" class="text-red-500 text-sm">{{ cepError }}</span>
        </div>

        <!-- Peso Inicial -->
        <div class="flex flex-col mx-4">
          <label for="weight_ini" class="mb-2 text-white">Peso Inicial <span class="text-red-500">*</span></label>
          <input type="number" id="weight_ini" placeholder="Digite seu peso inicial aqui..." v-model="weight_ini"
            class="w-full p-3 rounded focus:border-orange-500" required>
        </div>

        <!-- Altura Inicial -->
        <div class="flex flex-col mx-4">
          <label for="height_ini" class="mb-2 text-white">Altura Inicial <span class="text-red-500">*</span></label>
          <input type="number" id="height_ini" placeholder="Digite sua altura inicial aqui..." v-model="height_ini"
            class="w-full p-3 rounded focus:border-orange-500" required>
        </div>
      </div>

      <!-- Botões de envio e voltar -->
      <div class="flex justify-center mt-6 gap-4">
        <button type="button" @click="goBack" class="bg-gray-500 text-white p-3 rounded">Voltar</button>
        <button type="submit" class="bg-orange-500 text-white p-3 rounded">Cadastrar</button>
      </div>
    </form>
  </div>
</template>

<script>
import ToastComponent from '../components/ToastNotification.vue'

export default {
  components: {
    ToastComponent,
  },
  data() {
    return {
      fullName: '',
      cpf: '',
      dataNasc: '',
      gender: '',
      phone: '',
      email: '',
      userName: '',
      street: '',
      number: '',
      complement: '',
      district: '',
      city: '',
      state: '',
      cep: '',
      weight_ini: '',
      height_ini: '',
      showToast: false,
      toastMessage: '',
      toastType: '',
      cpfError: '',
      phoneError: '',
      emailError: '',
      cepError: ''
    };
  },
  methods: {
    handleRegister() {
      this.cpfError = '';
      this.phoneError = '';
      this.emailError = '';
      this.cepError = '';

      // Validação dos campos
      if (!this.isValidCPF(this.cpf)) {
        this.cpfError = 'CPF inválido.';
        return;
      }
      if (!this.isValidPhone(this.phone)) {
        this.phoneError = 'Telefone inválido.';
        return;
      }
      if (!this.isValidEmail(this.email)) {
        this.emailError = 'Email inválido.';
        return;
      }
      if (!this.isValidCEP(this.cep)) {
        this.cepError = 'CEP inválido.';
        return;
      }

      // Aqui você pode adicionar a lógica para enviar os dados para o backend
      // Após o sucesso, você pode mostrar uma mensagem de sucesso
      this.showToastMessage('Paciente cadastrado com sucesso!, uma senha provisória foi enviada para o e-mail', 'success');
    },
    isValidCPF(cpf) {
      // Lógica para validar CPF
      cpf = cpf.replace(/[^\d]+/g, ''); // Remove caracteres não numéricos
      if (cpf.length !== 11) return false;

      let soma = 0;
      let resto;

      for (let i = 1; i <= 9; i++) {
        soma += parseInt(cpf.charAt(i - 1)) * (11 - i);
      }
      resto = (soma * 10) % 11;

      if (resto === 10 || resto === 11) resto = 0;
      if (resto !== parseInt(cpf.charAt(9))) return false;

      soma = 0;
      for (let i = 1; i <= 10; i++) {
        soma += parseInt(cpf.charAt(i - 1)) * (12 - i);
      }
      resto = (soma * 10) % 11;

      if (resto === 10 || resto === 11) resto = 0;
      return resto === parseInt(cpf.charAt(10));
    },
    isValidPhone(phone) {
      // Lógica para validar telefone (considerando apenas números)
      phone = phone.replace(/[^\d]+/g, ''); // Remove caracteres não numéricos
      return phone.length >= 10 && phone.length <= 11; // Validar comprimento
    },
    isValidEmail(email) {
      // Lógica para validar email
      const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return re.test(email);
    },
    isValidCEP(cep) {
      // Lógica para validar CEP (considerando apenas números)
      cep = cep.replace(/[^\d]+/g, ''); // Remove caracteres não numéricos
      return cep.length === 8; // Validar comprimento
    },
    showToastMessage(message, type) {
      this.toastMessage = message;
      this.toastType = type;
      this.showToast = true;

      setTimeout(() => {
        this.showToast = false;
      }, 7000);
    },
    goBack() {
      // Implemente a lógica para voltar à página anterior
      window.history.back();
    }
  }
};
</script>

<style scoped>
/* Estilos personalizados aqui */
</style>
