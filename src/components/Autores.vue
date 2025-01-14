<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-6">
        <form @submit.prevent="crearAutor">
            
            <label for="id">id:</label>
            <input v-model="Author.Id" type="text" id="Id"><br>

            <label for="nombre">Nombre del Autor:</label>
            <input v-model="Author.Nombre" type="text" id="Nombre" required>

            <button type="submit">Crear Autor</button>
        
        </form>
        <button type="button" class="btn btn-primary" @click="EditarDatos()">Editar</button><br>
      </div><br>

        
      <div class="col-md-6">
          <table class="table table-bordered table-stripped table-hover">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Nombre del autor</th>
                  <th>Borrar</th>
                  <!-- Agrega más columnas según tus datos -->
                </tr>
              </thead>
              <tbody>
                <tr v-for="autor in authors" :key="autor" @click="cargarDatos(autor)">
                  <td>{{ autor.id }}</td>
                  <td>{{ autor.nombre }}</td>
                  <td>
                    <button class="btn btn-danger" @click="BorrarAutor(autor.id)">Eliminar</button>
                  </td>
                    <!-- Agrega más celdas según tus datos -->
                </tr>
              </tbody>
            </table>
        </div>
  </div>
</div>
</template>

<script setup>
import {ref, onMounted } from 'vue';
import axios from 'axios';
import 'bootstrap/dist/css/bootstrap.css';
const nombre = ref('');
const id = ref('');
const authors = ref([]);

const Author = ref({
  Id: '',
  Nombre : ''
});

const cargarDatos = (auth) => {
  // id.value = auth.id;
  // nombre.value = auth.nombre;

  Author.value.Id = auth.id;
  Author.value.Nombre = auth.nombre;
};

const crearAutor = async () => {
    try {
      const response = await axios.post('http://localhost:49309/api/Autores', { nombre: Author.value.Nombre })
      .then((response) => {
        console.log('Autor editado:', response.data);
        LlamarTabla();
      })
      .catch((error) => {
        console.error('Error al editar el autor:', error);
      });
      // Puedes redirigir al usuario a otra página o actualizar la lista de autores aquí
    } catch (error) {
      console.error('Error al crear el autor:', error);
    }
    
  };

  const EditarDatos = () => {
    if(Author.value.Id == ''){
      return false;
    }
    const miAutor = {
      Id: Author.value.Id,
      Nombre: Author.value.Nombre
    };
  
   try{
    axios.put('http://localhost:49309/api/Autores', miAutor)
      .then((response) => {
        console.log('Autor editado:', response.data);
        LlamarTabla();
      })
      .catch((error) => {
        console.error('Error al editar el autor:', error);
      });
   }catch(error){
    console.error('Error al editar el autor:', error);
   }
 
  };

const BorrarAutor = async (autorId) => {
  try {
    axios.delete(`http://localhost:49309/api/Autores/${autorId}`)
    .then((response) => {
        console.log('Autor eliminado correctamente:', response.data);
        LlamarTabla();
      })
      .catch((error) => {
        console.error('Error al eliminar el autor:', error);
      });
  } catch (error) {
    console.error('Error al eliminar el autor:', error);
  }
};

const LlamarTabla = async  () => {
  try {
    const response = await axios.get('http://localhost:49309/api/Autores');
    authors.value = response.data;
    console.log(response.data);
  } catch (error) {
    console.error('Error al obtener la lista de autores:', error);
  }
};

onMounted(async () => {
  LlamarTabla();
});




</script>


