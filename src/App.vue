<script setup>
import {ref, onMounted } from 'vue';
import axios from 'axios'

import TheWelcome from './components/TheWelcome.vue'
import Autores from './components/Autores.vue';
import Editoriales from './components/Editoriales.vue';
import Libros from './components/Libros.vue';

</script>

<template>    
      <Autores/>
      <Editoriales/>
      <Libros/>
  
</template>

<style scoped>

</style>
