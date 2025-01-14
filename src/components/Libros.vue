<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-6">
                <form @submit.prevent="crearLibro">

                    <label for="id">id:</label>
                    <input v-model="Libro.Id" type="text" id="Id"><br>

                    <label for="nombre">Nombre del Libro:</label>
                    <input v-model="Libro.Nombre" type="text" id="Nombre" required><br>

                    <label for="nombre">ISBN:</label>
                    <input v-model="Libro.ISBN" type="text" id="ISBN" required><br>

                    <label for="nombre">Id del autor:</label>
                    <input v-model="Libro.Autorid" type="text" id="Autord" required><br>

                    <label for="nombre">Id del Editorial:</label>
                    <input v-model="Libro.Editorialid" type="text" id="EditorialId" required>

                    <button type="submit">Crear Libro</button>
                </form>
                <button type="button" class="btn btn-primary" @click="editarLibro()">Editar</button>
            </div><br>

            <div class="col-md-6">
                <table class="table table-bordered table-stripped table-hover">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Nombre del Libro</th>
                            <th>ISBN</th>
                            <th>Autor</th>
                            <th>Editorial</th>
                            <th>Borrar</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="libro in libros" :key="libro" @click="cargarDatos(libro)">
                            <td>{{ libro.id }}</td>
                            <td>{{ libro.nombre }}</td>
                            <td>{{ libro.isbn }}</td>
                            <td>{{ libro.autor.nombre }}</td>
                            <td>{{ libro.editorial.nombre }}</td>
                            <td>
                                <button class="btn btn-danger" @click="borrarLibro(libro.id)">Eliminar</button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>
<script setup>
    import {ref, onMounted} from 'vue';
    import axios from 'axios';
    import 'bootstrap/dist/css/bootstrap.css';
    const nombre = ref('');
    const id = ref ('');
    const isbn = ref('')
    const autorid = ref ('');
    const editorialid = ref ('');
    const autordl = ref ('');
    const editorialdl = ref('');
    const libros = ref('');

    const Libro = ref({
        Id: '',
        Nombre: '',
        ISBN: '',
        Autorid: '',
        Autor: '',
        Editorialid: '',
        Editorial: ''
    });

    const cargarDatos = (lib) => {
        Libro.value.Id = lib.id;
        Libro.value.Nombre = lib.nombre;
        Libro.value.ISBN = lib.isbn;
        Libro.value.Autorid = lib.autorid;
        Libro.value.Editorialid = lib.editorialid;
    };

    const crearLibro = async () => {
    try {
      const response = await axios.post('http://localhost:49309/api/Libros', { Nombre: Libro.value.Nombre, ISBN: Libro.value.ISBN, Autorid: Libro.value.Autorid, Editorialid: Libro.value.Editorialid })
      .then((response) => {
        console.log('Libro creado:', response.data);
        LlamarTabla();
      })
      .catch((error) => {
        console.error('Error al crear el libro:', error);
      });
    } catch (error) {
      console.error('Error al crear el libro:', error);
    }
    
    };

    const borrarLibro = async (libroId) => {
        try {
            axios.delete(`http://localhost:49309/api/Libros/${libroId}`)
            .then ((response) => {
                console.log('Libro borrado correctamente', response.data);
                LlamarTabla();
            })
            .catch((error) => {
                console.error('Error al borrar el libro', error)
            })
        }catch (error) {
            console.error('Error al borrar el libro', error)
        }
    }

    const editarLibro = () => {
        if (Libro.value.Id == ''){
            return false;
        }
        const miLibro = {
            Id: Libro.value.Id,
            Nombre: Libro.value.Nombre,
            ISBN: Libro.value.ISBN,
            Autorid: Libro.value.Autorid,
            Editorialid: Libro.value.Editorialid
        };

        try{
            axios.put('http://localhost:49309/api/Libros', miLibro)
            .then((response) => {
                console.log('LibroEditado', response.data);
                LlamarTabla();
            })
            .catch((error) => {
                console.error('Error al editar', error);
            })
        } catch(error){
            console.error('Error al editar el libro', error);
        }
    }

    const LlamarTabla = async () => {
        try {
        const response = await axios.get('http://localhost:49309/api/Libros');
        libros.value = response.data;
        console.log(response.data);
        } catch (error) {
        console.error('Error al obtener la lista de editoriales:', error);
        }
    }

    onMounted(async () => {
    LlamarTabla();
    });
</script>