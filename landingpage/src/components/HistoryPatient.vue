<template>
  <div class="container mx-auto mt-10">
    <h1 class="text-3xl mb-12 text-white">HISTÓRICO DE AVALIAÇÕES</h1>

    <div class="overflow-x-auto justify-center">
      <div class="min-w-full bg-gray-800 shadow-md rounded-lg p-6">
        <table class="min-w-full divide-y divide-gray-700">
          <thead class="bg-gray-700">
            <tr>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                ID
              </th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                Data
              </th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                Peso
              </th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                Altura
              </th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                Método
              </th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                Ações
              </th>
            </tr>
          </thead>
          <tbody class="bg-gray-800 divide-y divide-gray-700">
            <tr v-for="assessment in assessments" :key="assessment.id_assessment">
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-300">
                {{ assessment.id_assessment }}
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-300">
                {{ new Date(assessment.assessmentDate).toLocaleDateString('pt-BR') }}
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-300">
                {{ assessment.weight }} kg
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-300">
                {{ assessment.height }} cm
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-300">
                {{ assessment.method }}
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-300">
                <GeneratePdf :user="user" :patient="patient" :assessments="[assessment]" />
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

  </div>
</template>

<script>
import GeneratePdf from '../../../frontend/src/components/GeneratePdf.vue';

export default {
  name: 'HistoryPatient',
  components: {
    GeneratePdf
  },
  data() {
    return {
      user: {
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
      },
      patient: {
        id_patient: 1,
        weigth_ini: 70,
        height_ini: 1.75
      },
      assessments: [
        {
          id_assessment: 1,
          assessmentDate: '2023-01-01',
          weight: 70,
          height: 175,
          method: 'Método Exemplo'
        },
        {
          id_assessment: 2,
          assessmentDate: '2023-02-01',
          weight: 72,
          height: 175,
          method: 'Método Exemplo'
        },
        {
          id_assessment: 3,
          assessmentDate: '2023-03-01',
          weight: 71,
          height: 175,
          method: 'Método Exemplo'
        }
      ]
    };
  },
  methods: {
    goBack() {
      this.$router.go(-1); // Navega de volta à página anterior
    }
  }
};
</script>

<style scoped>
.container {
  background-color: #1E2021;
  padding: 20px;
  border-radius: 8px;
}

h1 {
  color: #FF8139;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 12px;
  text-align: left;
}

th {
  background-color: #2D2D2D;
  color: #FF8139;
}

td {
  background-color: #1E2021;
  color: #FFFFFF;
}

tr:nth-child(even) td {
  background-color: #2D2D2D;
}
</style>