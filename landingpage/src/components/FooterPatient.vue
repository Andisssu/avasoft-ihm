<template>
    <div id="footer" class="flex flex-col md:flex-row md:items-center justify-center bg-[#131212] md:px-8 px-4 py-3 
    transition-transform duration-300" :class="{ 'ml-64': isSidebarOpen }" >

        <p class="text-sm text-gray-200 mb-4 mx-2 md:mb-0 text-center md:text-start">
            &copy; {{ year }} <span>Avasoft Company.</span> 
        </p>
        <p class="text-sm text-gray-200 mb-4 md:mb-0 text-center md:text-start md:mr-4">
            Todos os direitos reservados.
        </p>
    </div>
</template>
  
<script setup>
import { defineProps } from 'vue';

  let year = new Date().getFullYear();

  const props = defineProps({
  isSidebarOpen: Boolean
});
</script>
  
<style scoped></style>
  