<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-6">
                <form @submit.prevent="crearEditorial">

                    <label for="id">id:</label>
                    <input v-model="Editorial.Id" type="text" id="Id"><br>

                    <label for="nombre">Nombre del Editorial:</label>
                    <input v-model="Editorial.Nombre" type="text" id="Nombre" required>

                    <button type="submit">Crear Editorial</button>
                </form>
                <button type="button" class="btn btn-primary" @click="EditarEditorial()">Editar</button>
            </div><br>

            <div class="col-md-6">
                <table class="table table-bordered table-stripped table-hover">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Nombre del Editorial</th>
                            <th>Borrar</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="editorial in editoriales" :key="editorial" @click="cargarDatos(editorial)">
                            <td>{{ editorial.id }}</td>
                            <td>{{ editorial.nombre }}</td>
                            <td>
                                <button class="btn btn-danger" @click="BorrarEditorial(editorial.id)">Eliminar</button>
                            </td>
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
    const editoriales = ref([]);
    
    const Editorial = ref({
        Id: '',
        Nombre: ''
    });

    const cargarDatos = (edit) => {
        Editorial.value.Id = edit.id;
        Editorial.value.Nombre = edit.nombre;
    };

    const crearEditorial = async () => {
    try {
      const response = await axios.post('http://localhost:49309/api/Editoriales', { Nombre: Editorial.value.Nombre })
      .then((response) => {
        console.log('Editorial creado:', response.data);
        LlamarTabla();
      })
      .catch((error) => {
        console.error('Error al crear el editorial:', error);
      });
    } catch (error) {
      console.error('Error al crear el editorial:', error);
    }
    
    };

    const BorrarEditorial = async (editorialId) => {
        try {
            axios.delete(`http://localhost:49309/api/Editoriales/${editorialId}`)
            .then ((response) => {
                console.log('Editorial eliminado correctamente', response.data);
                LlamarTabla();
            })
            .catch((error) => {
                console.error('Error al crear el editorial', error);
            })
            } catch (error) {
                console.error('Error al eliminar el autor', error);
            }
    };

    const EditarEditorial = () => {
        if (Editorial.value.Id == ''){
            return false;
        }
        const miEditorial = {
            Id: Editorial.value.Id,
            Nombre: Editorial.value.Nombre
        };

        try{
            axios.put('http://localhost:49309/api/Editoriales', miEditorial)
            .then((response) => {
                console.log('Editorial editado', response.data);
                LlamarTabla();
            })
            .catch((error) => {
                console.error('Error al editar', error);
            })
        } catch(error){
            console.error('Error al editar el editorial', error);
        }
    }
    

  const LlamarTabla = async  () => {
  try {
    const response = await axios.get('http://localhost:49309/api/Editoriales');
    editoriales.value = response.data;
    console.log(response.data);
  } catch (error) {
    console.error('Error al obtener la lista de editoriales:', error);
  }
};

onMounted(async () => {
  LlamarTabla();
});
</script>