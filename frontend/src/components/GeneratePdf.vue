<template>
    <button type="button"
            class="bg-green-500 text-white py-1 px-2 inline-flex items-center gap-x-2 text-sm font-semibold rounded-lg border border-transparent"
            @click="generatePdf">
      Gerar PDF
    </button>
 
</template>

<script>
import jsPDF from 'jspdf';

export default {
  name: 'GeneratePdf',
  props: {
    user: Object,
    patient: Object,
    assessments: Array
  },
  methods: {
    generatePdf() {
      const doc = new jsPDF();

      // Adicionar título
      doc.setFontSize(18);
      doc.text('Informações do Paciente', 14, 22);

      // Adicionar dados do usuário em grid
      doc.setFontSize(12);
      const userInfo = [
        `Nome Completo: ${this.user.fullName}`,
        `CPF: ${this.user.cpf}`,
        `Data de Nascimento: ${this.user.dataNasc}`,
        `Gênero: ${this.user.gender}`,
        `Telefone: ${this.user.phone}`,
        `Email: ${this.user.email}`,
        `Endereço: ${this.user.street}, ${this.user.number} - ${this.user.complement}`,
        `Bairro: ${this.user.district}`,
        `Cidade: ${this.user.city}`,
        `Estado: ${this.user.state}`,
        `CEP: ${this.user.cep}`,
        `Nome de Usuário: ${this.user.userName}`
      ];

      let startY = 32;
      userInfo.forEach((info, index) => {
        const x = index % 2 === 0 ? 14 : 110; // Alternar entre duas colunas
        const y = startY + Math.floor(index / 2) * 8; // Incrementar a linha a cada duas entradas
        doc.text(info, x, y);
      });

      // Adicionar dados do paciente
      doc.setFontSize(14);
      doc.text('Dados do Paciente', 14, startY + Math.ceil(userInfo.length / 2) * 8 + 10);
      doc.setFontSize(12);
      doc.text(`Peso Inicial: ${this.patient.weigth_ini} kg`, 14, startY + Math.ceil(userInfo.length / 2) * 8 + 18);
      doc.text(`Altura Inicial: ${this.patient.height_ini} m`, 14, startY + Math.ceil(userInfo.length / 2) * 8 + 26);

      // Adicionar avaliações
      doc.setFontSize(14);
      doc.text('Avaliações', 14, startY + Math.ceil(userInfo.length / 2) * 8 + 46);
      this.assessments.forEach((assessment, index) => {
        const yOffset = startY + Math.ceil(userInfo.length / 2) * 8 + 54 + (index * 40); // Ajustar espaçamento entre avaliações
        doc.setFontSize(12);
        doc.text(`Avaliação ${index + 1}:`, 14, yOffset);
        doc.text(`Data: ${new Date(assessment.assessmentDate).toLocaleDateString('pt-BR')}`, 14, yOffset + 8);
        doc.text(`Peso: ${assessment.weight} kg`, 14, yOffset + 16);
        doc.text(`Altura: ${assessment.height} cm`, 14, yOffset + 24);
        doc.text(`Método: ${assessment.method}`, 14, yOffset + 32);
      });

      // Baixar o PDF
      doc.save(`Paciente_${this.patient.id_patient}.pdf`);
    }
  }
};
</script>

<style scoped>
button {
  margin: 5px;
}
</style>