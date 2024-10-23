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
                  <button @click="generatePdf(assessment)" class="bg-blue-500 text-white py-1 px-2 rounded-lg hover:bg-blue-600">
                    Gerar PDF
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
  
      <!-- Botão de Voltar -->
      <div class="mt-6 flex justify-end">
        <button @click="goBack" class="bg-[#FF8139] text-white px-4 py-2 rounded-md hover:bg-[#FF5C00]">
          Voltar
        </button>
      </div>
    </div>
  </template>
  
  <script>

  
  export default {
    name: 'HistoryPatient',
    data() {
      return {
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
      },
      generatePdf(assessment) {
        const doc = new jsPDF();
        doc.text('Avaliação do Paciente', 14, 16);
        doc.autoTable({
          startY: 20,
          head: [['ID', 'Data', 'Peso', 'Altura', 'Método']],
          body: [
            [
              assessment.id_assessment,
              new Date(assessment.assessmentDate).toLocaleDateString('pt-BR'),
              `${assessment.weight} kg`,
              `${assessment.height} cm`,
              assessment.method
            ]
          ]
        });
        doc.save(`avaliacao_${assessment.id_assessment}.pdf`);
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