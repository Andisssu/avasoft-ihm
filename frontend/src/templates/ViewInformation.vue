<template>
  <ToastComponent v-if="showToast" :message="toastMessage" :type="toastType" />
  <div class="p-6 min-h-screen">
    <h1 class="text-3xl font-bold text-center mb-6">Informações do Paciente</h1>

    <div v-if="patient && user" class="bg-white shadow-md rounded-lg p-6">
      <div v-if="patient && user" class="bg-white shadow-md rounded-lg p-6">
        <h2 class="text-2xl font-semibold text-gray-800 mb-4">Dados do Usuário</h2>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <p><strong>Nome Completo:</strong> {{ user.fullName }}</p>
          <p><strong>CPF:</strong> {{ user.cpf }}</p>
          <p><strong>Data de Nascimento:</strong> {{ user.dataNasc }}</p>
          <p><strong>Gênero:</strong> {{ user.gender }}</p>
          <p><strong>Telefone:</strong> {{ user.phone }}</p>
          <p><strong>Email:</strong> {{ user.email }}</p>
          <p><strong>Endereço:</strong> {{ user.street }}, {{ user.number }} - {{ user.complement }}</p>
          <p><strong>Bairro:</strong> {{ user.district }}</p>
          <p><strong>Cidade:</strong> {{ user.city }}</p>
          <p><strong>Estado:</strong> {{ user.state }}</p>
          <p><strong>CEP:</strong> {{ user.cep }}</p>
          <p><strong>Nome de Usuário:</strong> {{ user.userName }}</p>
        </div>

        <h2 class="text-2xl font-semibold text-gray-800 mt-6 mb-4">Dados do Paciente</h2>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <p><strong>Peso Inicial:</strong> {{ patient.weigth_ini }} kg</p>
          <p><strong>Altura Inicial:</strong> {{ patient.height_ini }} m</p>
        </div>
      </div>

      <!-- Tabela de avaliações -->
      <h2 class="text-2xl font-semibold text-gray-800 mt-6 mb-4">Avaliações</h2>
      <div class="flex flex-col">
        <div class="-m-1.5 overflow-x-auto">
          <div class="p-1.5 min-w-full inline-block align-middle">
            <div class="border rounded-lg shadow overflow-hidden dark:shadow-gray-400">
              <table class="min-w-full divide-y divide-gray-200 dark:divide-neutral-400">
                <thead class="bg-gray-100">
                  <tr>
                    <th scope="col"
                      class="px-6 py-3 text-start text-xs font-medium text-gray-900 uppercase dark:text-neutral-500">
                      ID</th>
                    <th scope="col"
                      class="px-6 py-3 text-start text-xs font-medium text-gray-900 uppercase dark:text-neutral-500">
                      Data</th>
                    <th scope="col"
                      class="px-6 py-3 text-end text-xs font-medium text-gray-900 uppercase dark:text-neutral-500">
                      Ações
                    </th>
                  </tr>
                </thead>
                <tbody class="divide-y divide-gray-200 dark:divide-neutral-400">
                  <tr v-for="assessment in filteredAssessments" :key="assessment.id_assessment">
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-800 dark:text-neutral-500">
                      {{ assessment.id_assessment }} </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-800 dark:text-neutral-500">
                      {{ new Date(assessment.assessmentDate).toLocaleDateString('pt-BR') }} </td>
                    <td class="px-6 py-4 whitespace-nowrap text-end text-sm font-medium">
                      <button type="button"
                        class="bg-blue-500 text-white py-1 px-2 inline-flex items-center gap-x-2 text-sm font-semibold rounded-lg border border-transparent"
                        aria-haspopup="dialog" aria-expanded="false" aria-controls="hs-large-modal"
                        data-hs-overlay="#hs-large-modal" @click="openModal(assessment)">
                        Visualizar
                      </button>
                      <button type="button" @click="editpage"
                        class="bg-yellow-500 text-white py-1 px-2 inline-flex items-center gap-x-2 text-sm font-semibold rounded-lg border border-transparent">Editar</button>
                      <button type="button" @click="openDeleteModal(assessment)"
                        class="bg-red-500 text-white py-1 px-2 inline-flex items-center gap-x-2 text-sm font-semibold rounded-lg border border-transparent"
                        aria-haspopup="dialog" aria-expanded="false" aria-controls="hs-danger-alert"
                        data-hs-overlay="#hs-danger-alert">Excluir</button>

                      <GeneratePdf :user="user" :patient="patient" :assessments="assessments" />
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>

      <!-- Modal de confirmação de exclusão -->
      <div id="hs-danger-alert"
        class="hs-overlay hidden size-full fixed top-0 start-0 z-[80] overflow-x-hidden overflow-y-auto" role="dialog"
        tabindex="-1" aria-labelledby="hs-danger-alert-label">
        <div
          class="hs-overlay-open:mt-7 hs-overlay-open:opacity-100 hs-overlay-open:duration-500 mt-0 opacity-0 ease-out transition-all md:max-w-2xl md:w-full m-3 md:mx-auto">
          <div
            class="relative flex flex-col bg-white border shadow-sm rounded-xl overflow-hidden dark:bg-neutral-900 dark:border-neutral-800">
            <div class="absolute top-2 end-2">
              <button type="button"
                class="size-8 inline-flex justify-center items-center gap-x-2 rounded-full border border-transparent bg-gray-100 text-gray-800 hover:bg-gray-200 focus:outline-none focus:bg-gray-200 disabled:opacity-50 disabled:pointer-events-none dark:bg-neutral-700 dark:hover:bg-neutral-600 dark:text-neutral-400 dark:focus:bg-neutral-600"
                aria-label="Close" data-hs-overlay="#hs-danger-alert" @click="cancelDelete">
                <span class="sr-only">Close</span>
                <svg class="shrink-0 size-4" xmlns="http://www.w3.org/2000/svg" width="24" height="24"
                  viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                  stroke-linejoin="round">
                  <path d="M18 6 6 18" />
                  <path d="m6 6 12 12" />
                </svg>
              </button>
            </div>
            <div class="p-4 sm:p-10 overflow-y-auto bg-gray-50">
              <div class="flex gap-x-4 md:gap-x-7">
                <span
                  class="shrink-0 inline-flex justify-center items-center size-[46px] sm:w-[62px] sm:h-[62px] rounded-full border-4 border-red-50 bg-red-100 text-red-500">
                  <svg class="shrink-0 size-5" xmlns="http://www.w3.org/2000/svg" width="16" height="16"
                    fill="currentColor" viewBox="0 0 16 16">
                    <path
                      d="M8.982 1.566a1.13 1.13 0 0 0-1.96 0L.165 13.233c-.457.778.091 1.767.98 1.767h13.713c.889 0 1.438-.99.98-1.767L8.982 1.566zM8 5c.535 0 .954.462.9.995l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 5.995A.905.905 0 0 1 8 5zm.002 6a1 1 0 1 1 0 2 1 1 0 0 1 0-2z" />
                  </svg>
                </span>
                <div class="grow">
                  <h3 id="hs-danger-alert-label" class="mb-2 text-xl font-bold text-gray-800">Excluir Avaliação</h3>
                  <p class="text-gray-500">Tem certeza que deseja excluir esta avaliação? Essa ação não poderá ser
                    desfeita.</p>
                </div>
              </div>
            </div>
            <div class="flex justify-end items-center gap-x-2 py-3 px-4 bg-gray-50 border-t">
              <button @click="cancelDelete" type="button"
                class="py-2 px-3 inline-flex items-center gap-x-2 text-sm font-medium rounded-lg border border-gray-200 bg-white text-gray-800 shadow-sm hover:bg-gray-50 disabled:opacity-50 disabled:pointer-events-none focus:outline-none focus:bg-gray-50"
                aria-label="Close" data-hs-overlay="#hs-danger-alert">Cancelar</button>
              <button @click="confirmDeleteAssessment" type="button"
                class="py-2 px-3 inline-flex items-center gap-x-2 text-sm font-medium rounded-lg border border-transparent bg-red-500 text-white hover:bg-red-600 disabled:opacity-50 disabled:pointer-events-none"
                aria-label="Close" data-hs-overlay="#hs-danger-alert">Excluir</button>
            </div>
          </div>
        </div>
      </div>

      <!-- Modal de visualização -->
      <div id="hs-large-modal"
        class="hs-overlay hidden size-full fixed top-0 start-0 z-[80] overflow-x-hidden overflow-y-auto" role="dialog"
        tabindex="-1" aria-labelledby="hs-large-modal-label">
        <div
          class="hs-overlay-open:mt-7 hs-overlay-open:opacity-100 hs-overlay-open:duration-500 mt-0 opacity-0 ease-out transition-all lg:max-w-4xl lg:w-full m-3 h-[calc(100%-3.5rem)] lg:mx-auto">
          <div
            class="flex flex-col max-h-full overflow-hidden bg-white border shadow-sm rounded-xl pointer-events-auto">
            <div class="flex justify-between items-center py-3 px-4 border-b">
              <h3 id="hs-large-modal-label" class="font-bold text-gray-800">
                Avaliação {{ currentAssessment?.id_assessment }}
              </h3>
              <button type="button"
                class="size-8 inline-flex justify-center items-center gap-x-2 rounded-full border border-transparent bg-gray-100 text-gray-800 hover:bg-gray-200 focus:outline-none focus:bg-gray-200 disabled:opacity-50 disabled:pointer-events-none"
                aria-label="Close" data-hs-overlay="#hs-large-modal" @click="closeModal">
                <span class="sr-only">Close</span>
                <svg class="shrink-0 size-4" xmlns="http://www.w3.org/2000/svg" width="24" height="24"
                  viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                  stroke-linejoin="round">
                  <path d="M18 6 6 18"></path>
                  <path d="m6 6 12 12"></path>
                </svg>
              </button>
            </div>

            <div class="p-4 overflow-y-auto" v-if="currentAssessment">
              <div class="grid grid-cols-2 gap-4">
                <p><strong>Data da Avaliação:</strong> {{ (currentAssessment.assessmentDate) }}</p>
                <p><strong>Peso:</strong> {{ currentAssessment.weight }} kg</p>
                <p><strong>Altura:</strong> {{ currentAssessment.height }} cm</p>
                <p><strong>Método:</strong> {{ currentAssessment.method }}</p>
              </div>

              <h4 class="my-4 font-bold text-black">Circunferências (cm)</h4>
              <div class="grid grid-cols-2 gap-4">
                <p><strong>Pescoço:</strong> {{ currentAssessment.circumference.neck }} cm</p>
                <p><strong>Tórax:</strong> {{ currentAssessment.circumference.thorax }} cm</p>
                <p><strong>Cintura:</strong> {{ currentAssessment.circumference.waist }} cm</p>
                <p><strong>Abdômen:</strong> {{ currentAssessment.circumference.abdomen }} cm</p>
                <!-- Continue para as outras medidas -->
              </div>

              <h4 class="my-4 font-bold text-black">Dobras Cutâneas (mm)</h4>
              <div class="grid grid-cols-2 gap-4">
                <p><strong>Tríceps:</strong> {{ currentAssessment.skinfold.triceps }} mm</p>
                <p><strong>Abdominal:</strong> {{ currentAssessment.skinfold.abdominal }} mm</p>
                <!-- Continue para as outras dobras -->
              </div>

              <!-- Exibir IMC e Composição Corporal, se disponível -->
              <template v-if="currentAssessment.method !== 'Dados Livres'">
                <h4 class="mt-4 font-bold">IMC</h4>
                <p v-if="currentAssessment.bmi"><strong>Valor do IMC:</strong> {{ currentAssessment.bmi.bmiValue }}</p>
                <p v-if="currentAssessment.bmi"><strong>Classificação:</strong> {{ currentAssessment.bmi.classification
                  }}
                </p>

                <h4 class="mt-4 font-bold">Composição Corporal</h4>
                <p v-if="currentAssessment.bodyComposition"><strong>Densidade Corporal:</strong> {{
                  currentAssessment.bodyComposition.body_density }}</p>
                <p v-if="currentAssessment.bodyComposition"><strong>Percentual de Gordura Corporal:</strong> {{
                  currentAssessment.bodyComposition.body_fat_percentage }}%</p>
              </template>
            </div>

            <div class="flex justify-end items-center gap-x-2 py-3 px-4 border-t">
              <button type="button"
                class="py-2 px-3 inline-flex items-center gap-x-2 text-sm font-medium rounded-lg border border-gray-200 bg-white text-gray-800 shadow-sm hover:bg-gray-50 focus:outline-none focus:bg-gray-50 disabled:opacity-50 disabled:pointer-events-none"
                aria-label="Close" data-hs-overlay="#hs-large-modal" @click="closeModal">
                Fechar
              </button>
            </div>
          </div>
        </div>
      </div>

    </div>


    <div class="mt-6 flex justify-between">
      <button @click="goBack"
        class="bg-blue-500 text-white py-2 px-4 rounded-lg hover:bg-blue-600 transition duration-200">
        Voltar
      </button>
      <div>
        <button @click="EditaData"
          class="bg-yellow-500 text-white py-2 px-4 rounded-lg hover:bg-yellow-600 transition duration-200 mr-2"
          aria-haspopup="dialog" aria-expanded="false" aria-controls="hs-scale-animation-modal"
          data-hs-overlay="#hs-scale-animation-modal">
          Editar Dados
        </button>
        <button @click="AssessmentPage(patient.id_patient)"
          class="bg-orange-500 text-white py-2 px-4 rounded-lg hover:bg-indigo-600 transition duration-200">
          Nova Avaliação
        </button>
      </div>
    </div>

    <!-- Modal de edição de paciente -->
    <div id="hs-scale-animation-modal"
      class="hs-overlay hidden size-full fixed top-0 start-0 z-[80] overflow-x-hidden overflow-y-auto pointer-events-none"
      role="dialog" tabindex="-1" aria-labelledby="hs-scale-animation-modal-label">
      <div
        class="hs-overlay-open:mt-7 hs-overlay-open:opacity-100 hs-overlay-open:duration-500 mt-0 opacity-0 ease-out transition-all lg:max-w-4xl lg:w-full m-3 lg:mx-auto">
        <div
          class="flex flex-col bg-white border shadow-sm rounded-xl pointer-events-auto dark:bg-neutral-800 dark:border-neutral-700 dark:shadow-neutral-700/70">
          <div class="flex justify-between items-center py-3 px-4 border-b dark:border-neutral-700">
            <h3 id="hs-large-modal-label" class="font-bold text-gray-800 dark:text-white">
              Editar Dados do Paciente
            </h3>
            <button type="button"
              class="size-8 inline-flex justify-center items-center gap-x-2 rounded-full border border-transparent bg-gray-100 text-gray-800 hover:bg-gray-200 focus:outline-none focus:bg-gray-200 disabled:opacity-50 disabled:pointer-events-none dark:bg-neutral-700 dark:hover:bg-neutral-600 dark:text-neutral-400 dark:focus:bg-neutral-600"
              aria-label="Close" data-hs-overlay="#hs-scale-animation-modal">
              <span class="sr-only">Fechar</span>
              <svg class="shrink-0 size-4" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
                fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M18 6 6 18"></path>
                <path d="m6 6 12 12"></path>
              </svg>
            </button>
          </div>

          <form id="edit-patient-form" class="p-4 overflow-y-auto">
            <div class="grid grid-cols-1 gap-4 sm:grid-cols-2">
              <!-- Nome Completo -->
              <div>
                <label for="fullName" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Nome
                  Completo</label>
                <input type="text" id="fullName" v-model="patientToEdit.fullName"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- CPF -->
              <div>
                <label for="cpf" class="block text-sm font-medium text-gray-700 dark:text-gray-300">CPF</label>
                <input type="text" id="cpf" v-model="patientToEdit.cpf"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- Data de Nascimento -->
              <div>
                <label for="dataNasc" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Data de
                  Nascimento</label>
                <input type="date" id="dataNasc" v-model="patientToEdit.dataNasc"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- Gênero -->
              <div>
                <label for="gender" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Gênero</label>
                <select id="gender" v-model="patientToEdit.gender"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
                  <option value="Masculino">Masculino</option>
                  <option value="Feminino">Feminino</option>
                  <option value="Outro">Outro</option>
                </select>
              </div>
              <!-- Telefone -->
              <div>
                <label for="phone" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Telefone</label>
                <input type="text" id="phone" v-model="patientToEdit.phone"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- Email -->
              <div>
                <label for="email" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Email</label>
                <input type="email" id="email" v-model="patientToEdit.email"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- Endereço -->
              <div>
                <label for="street" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Rua</label>
                <input type="text" id="street" v-model="patientToEdit.street"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- Número -->
              <div>
                <label for="number" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Número</label>
                <input type="number" id="number" v-model="patientToEdit.number"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- Complemento -->
              <div>
                <label for="complement"
                  class="block text-sm font-medium text-gray-700 dark:text-gray-300">Complemento</label>
                <input type="text" id="complement" v-model="patientToEdit.complement"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm">
              </div>
              <!-- Bairro -->
              <div>
                <label for="district" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Bairro</label>
                <input type="text" id="district" v-model="patientToEdit.district"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- Cidade -->
              <div>
                <label for="city" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Cidade</label>
                <input type="text" id="city" v-model="patientToEdit.city"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- Estado -->
              <div>
                <label for="state" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Estado</label>
                <input type="text" id="state" v-model="patientToEdit.state"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- CEP -->
              <div>
                <label for="cep" class="block text-sm font-medium text-gray-700 dark:text-gray-300">CEP</label>
                <input type="text" id="cep" v-model="patientToEdit.cep"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
                  required>
              </div>
              <!-- Peso Inicial -->
              <div>
                <label for="weigth_ini" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Peso
                  Inicial</label>
                <input type="number" id="weigth_ini" v-model="patientToEdit.weigth_ini"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm">
              </div>
              <!-- Altura Inicial -->
              <div>
                <label for="height_ini" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Altura
                  Inicial</label>
                <input type="number" id="height_ini" v-model="patientToEdit.height_ini"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm">
              </div>
            </div>
          </form>

          <div class="flex justify-end items-center gap-x-2 py-3 px-4 border-t dark:border-neutral-700">
            <button type="button"
              class="py-2 px-3 inline-flex items-center gap-x-2 text-sm font-medium rounded-lg border border-gray-200 bg-white text-gray-800 shadow-sm hover:bg-gray-50 focus:outline-none focus:bg-gray-50 disabled:opacity-50 disabled:pointer-events-none dark:bg-neutral-800 dark:border-neutral-700 dark:text-white dark:hover:bg-neutral-700 dark:focus:bg-neutral-700"
              aria-label="Close" data-hs-overlay="#hs-scale-animation-modal">
              Cancelar
            </button>
            <button @click="updatePatient" type="submit"
              class="py-2 px-3 inline-flex items-center gap-x-2 text-sm font-medium rounded-lg text-white bg-blue-600 shadow-sm hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:outline-none focus:ring-offset-white dark:focus:ring-offset-neutral-800">
              Atualizar
            </button>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>


<script>
import ToastComponent from '../components/ToastNotification.vue';
import GeneratePdf from '../components/GeneratePdf.vue';

export default {
  name: 'ViewInformation',
  components: {
    ToastComponent,
    GeneratePdf
  },
  data() {
    return {
      patient: null,
      user: null,
      showEditModal: false,
      toastMessage: '',
      toastType: 'success',
      showToast: false,
      selectedAssessment: null,
      patientToEdit: {
        user_id: '',
        fullName: '',
        cpf: '',
        dataNasc: '',
        phone: '',
        email: '',
        street: '',
      },
      assessments: [],
      currentAssessment: null,
      assessmentToDelete: null,
      deletedAssessments: []
    };
  },
  methods: {
    openModal(assessment) {
      this.currentAssessment = assessment; // Define a avaliação selecionada
    },
    closeModal() {
      this.currentAssessment = null; // Limpa os dados ao fechar o modal
    },
    goBack() {
      this.$router.push('/patientlist');
    },
    AssessmentPage(id) {
      this.$router.push(`/AssessmentPage/${id}`);
    },
    editpage() {
      this.$router.push('/EditAssessment');
    },
    EditaData() {

    },
    showToastMessage(message, type) {
      this.toastMessage = message;
      this.toastType = type;
      this.showToast = false;
      this.$nextTick(() => {
        this.showToast = true;
      });
    },
    openDeleteModal(assessment) {
      this.assessmentToDelete = assessment;
      this.showModal = true;
    },
    confirmDeleteAssessment() {
      if (this.assessmentToDelete) {
        this.deletedAssessments.push(this.assessmentToDelete.id_assessment);
        this.showToastMessage('Avaliação excluída com sucesso', 'success');
        this.cancelDelete();
      }
    },
    cancelDelete() {
      this.assessmentToDelete = null;
      this.showModal = false;
    }
  },
  computed: {
    filteredAssessments() {
      return this.assessments.filter(assessment => !this.deletedAssessments.includes(assessment.id_assessment));
    }
  },
  mounted() {
    // Dados fictícios para exibição
    this.user = {
      fullName: 'João da Silva',
      cpf: '123.456.789-00',
      dataNasc: '01/01/1980',
      gender: 'Masculino',
      phone: '(11) 98765-4321',
      email: 'joao.silva@example.com',
      street: 'Rua Exemplo',
      number: '123',
      complement: 'Apto 45',
      district: 'Bairro Exemplo',
      city: 'Cidade Exemplo',
      state: 'Estado Exemplo',
      cep: '12345-678',
      userName: 'joaosilva'
    };

    this.patient = {
      weigth_ini: 70,
      height_ini: 1.75
    };

    // Dados fictícios para avaliações
    this.assessments = [
      {
        id_assessment: 1,
        assessmentDate: '2023-01-01',
        weight: 70,
        height: 175,
        method: 'Método Exemplo',
        circumference: {
          neck: 40,
          thorax: 100,
          waist: 80,
          abdomen: 85,
          hip: 95,
          leftArm: 30,
          rightArm: 30,
          leftForearm: 25,
          rightForearm: 25,
          leftGlutealThigh: 50,
          rightGlutealThigh: 50,
          leftLeg: 35,
          rightLeg: 35,
          leftWrist: 15,
          rightWrist: 15
        },
        skinfold: {
          triceps: 10,
          abdominal: 20,
          subscapular: 15,
          suprailiac: 12,
          thigh: 18
        },
        bmi: {
          bmiValue: 22.9,
          classification: 'Normal'
        },
        bodyComposition: {
          body_density: 1.05,
          body_fat_percentage: 15
        }
      },
      {
        id_assessment: 2,
        assessmentDate: '2023-01-01',
        weight: 70,
        height: 175,
        method: 'Método Exemplo',
        circumference: {
          neck: 40,
          thorax: 100,
          waist: 80,
          abdomen: 85,
          hip: 95,
          leftArm: 30,
          rightArm: 30,
          leftForearm: 25,
          rightForearm: 25,
          leftGlutealThigh: 50,
          rightGlutealThigh: 50,
          leftLeg: 35,
          rightLeg: 35,
          leftWrist: 15,
          rightWrist: 15
        },
        skinfold: {
          triceps: 10,
          abdominal: 20,
          subscapular: 15,
          suprailiac: 12,
          thigh: 18
        },
        bmi: {
          bmiValue: 22.9,
          classification: 'Normal'
        },
        bodyComposition: {
          body_density: 1.05,
          body_fat_percentage: 15
        }
      },
      {
        id_assessment: 3,
        assessmentDate: '2023-01-01',
        weight: 70,
        height: 175,
        method: 'Método Exemplo',
        circumference: {
          neck: 40,
          thorax: 100,
          waist: 80,
          abdomen: 85,
          hip: 95,
          leftArm: 30,
          rightArm: 30,
          leftForearm: 25,
          rightForearm: 25,
          leftGlutealThigh: 50,
          rightGlutealThigh: 50,
          leftLeg: 35,
          rightLeg: 35,
          leftWrist: 15,
          rightWrist: 15
        },
        skinfold: {
          triceps: 10,
          abdominal: 20,
          subscapular: 15,
          suprailiac: 12,
          thigh: 18
        },
        bmi: {
          bmiValue: 22.9,
          classification: 'Normal'
        },
        bodyComposition: {
          body_density: 1.05,
          body_fat_percentage: 15
        }
      }
    ];
  },
};
</script>

<style scoped>
button {
  margin: 5px;
}
</style>