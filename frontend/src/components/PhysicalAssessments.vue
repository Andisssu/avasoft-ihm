<template>
  <ToastComponent v-if="showToast" :message="toastMessage" :type="toastType" />
  <div class="flex flex-col justify-start items-center min-h-screen px-4">

    <!-- Título da página -->
    <h2 class="text-2xl mb-6">Listagem de Avaliações Físicas</h2>
    <div class="container mx-auto mt-10">

      <div class="overflow-x-auto">
        <table class="min-w-full" style="border-collapse: separate; border-spacing: 0 20px;">
          <thead>
            <tr class="bg-orange-500 text-white">
              <th class="py-2 px-4 text-start rounded-l-lg">ID</th>
              <th class="py-2 px-4 text-start">Data</th>
              <th class="py-2 px-4 text-start">Paciente</th>
              <th class="py-2 px-4 text-center rounded-r-lg">Ações</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="assessment in assessments" :key="assessment.id_assessment" class="bg-gray-700 text-white">
              <td class="py-2 px-4 rounded-l-lg">{{ assessment.id_assessment }}</td>
              <td class="py-2 px-4">{{ formatDate(assessment.assessmentDate) }}</td>
              <td class="py-2 px-4">{{ assessment.patient.user.fullName }}</td>
              <td class="py-2 px-4 text-center rounded-r-lg flex justify-center space-x-2">
                <button type="button"
                  class="bg-blue-500 text-white py-1 px-2 inline-flex items-center gap-x-2 text-sm font-semibold rounded-lg border border-transparent"
                  aria-haspopup="dialog" aria-expanded="false" aria-controls="hs-large-modal"
                  data-hs-overlay="#hs-large-modal" @click="openModal(assessment)">
                  Visualizar
                </button>
                <button class="bg-yellow-500 text-white py-1 px-2 rounded " @click="editpage">Editar</button>
                <button class="bg-red-500 text-white py-1 px-2 rounded " 
                        aria-haspopup="dialog" aria-expanded="false" aria-controls="hs-danger-alert"
                        data-hs-overlay="#hs-danger-alert"
                  @click="openDeleteModal(assessment)" >Excluir</button>
              </td>
            </tr>
          </tbody>
        </table>
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
        <div class="flex flex-col max-h-full overflow-hidden bg-white border shadow-sm rounded-xl pointer-events-auto">
          <div class="flex justify-between items-center py-3 px-4 border-b">
            <h3 id="hs-large-modal-label" class="font-bold text-gray-800">
              Avaliação {{ currentAssessment?.id_assessment }}
            </h3>
            <button type="button"
              class="size-8 inline-flex justify-center items-center gap-x-2 rounded-full border border-transparent bg-gray-100 text-gray-800 hover:bg-gray-200 focus:outline-none focus:bg-gray-200 disabled:opacity-50 disabled:pointer-events-none"
              aria-label="Close" data-hs-overlay="#hs-large-modal" @click="closeModal">
              <span class="sr-only">Close</span>
              <svg class="shrink-0 size-4" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
                fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M18 6 6 18"></path>
                <path d="m6 6 12 12"></path>
              </svg>
            </button>
          </div>

          <div class="p-4 overflow-y-auto" v-if="currentAssessment">
            <div class="grid grid-cols-2 gap-4">
              <p><strong>Data da Avaliação:</strong> {{ formatDate(currentAssessment.assessmentDate) }}</p>
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
              <p v-if="currentAssessment.bmi"><strong>Classificação:</strong> {{ currentAssessment.bmi.classification }}
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
</template>

<script>
import ToastComponent from '../components/ToastNotification.vue';
export default {
  components: {
    ToastComponent,
  },
  data() {
    return {
      assessments: [
        {
          id_assessment: 1,
          assessmentDate: '2023-01-01',
          patient: { user: { fullName: 'Paciente 1' } },
          weight: 70,
          height: 170,
          method: 'Método 1',
          circumference: {
            neck: 35,
            thorax: 90,
            waist: 80,
            abdomen: 85,
            hip: 95,
            leftArm: 30,
            rightArm: 30,
            leftForearm: 25,
            rightForearm: 25,
            leftGlutealThigh: 50,
            rightGlutealThigh: 50,
            leftLeg: 40,
            rightLeg: 40,
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
            bmiValue: 24.2,
            classification: 'Normal'
          },
          bodyComposition: {
            body_density: 1.05,
            body_fat_percentage: 15
          }
        },
        {
          id_assessment: 2,
          assessmentDate: '2023-02-01',
          patient: { user: { fullName: 'Paciente 2' } },
          weight: 80,
          height: 180,
          method: 'Método 2',
          circumference: {
            neck: 40,
            thorax: 100,
            waist: 90,
            abdomen: 95,
            hip: 105,
            leftArm: 35,
            rightArm: 35,
            leftForearm: 30,
            rightForearm: 30,
            leftGlutealThigh: 55,
            rightGlutealThigh: 55,
            leftLeg: 45,
            rightLeg: 45,
            leftWrist: 20,
            rightWrist: 20
          },
          skinfold: {
            triceps: 12,
            abdominal: 22,
            subscapular: 17,
            suprailiac: 14,
            thigh: 20
          },
          bmi: {
            bmiValue: 24.7,
            classification: 'Normal'
          },
          bodyComposition: {
            body_density: 1.06,
            body_fat_percentage: 16
          }
        },
        {
          id_assessment: 3,
          assessmentDate: '2023-03-01',
          patient: { user: { fullName: 'Paciente 3' } },
          weight: 90,
          height: 190,
          method: 'Método 3',
          circumference: {
            neck: 45,
            thorax: 110,
            waist: 100,
            abdomen: 105,
            hip: 115,
            leftArm: 40,
            rightArm: 40,
            leftForearm: 35,
            rightForearm: 35,
            leftGlutealThigh: 60,
            rightGlutealThigh: 60,
            leftLeg: 50,
            rightLeg: 50,
            leftWrist: 25,
            rightWrist: 25
          },
          skinfold: {
            triceps: 14,
            abdominal: 24,
            subscapular: 19,
            suprailiac: 16,
            thigh: 22
          },
          bmi: {
            bmiValue: 24.9,
            classification: 'Normal'
          },
          bodyComposition: {
            body_density: 1.07,
            body_fat_percentage: 17
          }
        }
      ],
      toastMessage: '',
      toastType: 'success',
      showModal: false, // Adicione esta linh
      currentAssessment: null,
      assessmentToDelete: null,
      deletedAssessments: []
    };
  },
  methods: {
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
        // Remover a avaliação da lista
        this.assessments = this.assessments.filter(assessment => assessment.id_assessment !== this.assessmentToDelete.id_assessment);
        this.showToastMessage('Avaliação excluída com sucesso', 'success');
        this.cancelDelete();
      }
    },
    cancelDelete() {
      this.assessmentToDelete = null;
      this.showModal = false;
    },
    openModal(assessment) {
      this.currentAssessment = assessment; // Define a avaliação selecionada
    },
    closeModal() {
      this.currentAssessment = null; // Limpa os dados ao fechar o modal
    },
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      return new Date(date).toLocaleDateString('pt-BR', options);
    }
  }
};

document.addEventListener('DOMContentLoaded', function () {
  // Inicializar o Preline para aplicar o comportamento overlay e animações
  window.hsOverlay?.init();
});
</script>

<style>
.disabled {
  pointer-events: none;
  opacity: 0.5;
  cursor: not-allowed;
}

#btnnew {
  background-color: #707070;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 4px;
  width: 100%;
}

@media (min-width: 768px) {
  #btnnew {
    width: auto;
  }
}
</style>
