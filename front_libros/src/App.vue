<script setup>
import{ ref, onMounted } from 'vue';
import axios from 'axios';

const libros = ref([]);

const nuevoLibro = ref({
  titulo: '',
  autor: '',
  categoria: '',
  editorial: '',
  nump: '',
  descripcion: '',
  imagenURL: ''
})

const editado = ref(false);


const cargarLibro = async () => {
  const response = await axios.get('http://localhost:8088/libro/traer-libro');
  libros.value = response.data;
  console.log(libros.value);
}

const agregarLibro = async () => {
  if(editado.value){
    await axios.put(`http://localhost:8088/libro/editar-libro/${nuevoLibro.value.id}`, nuevoLibro.value);
    editado.value = false
  }else{
    await axios.post('http://localhost:8088/libro/insertar-libro', nuevoLibro.value);
  }

  await cargarLibro();
  nuevoLibro.value = {
    titulo: '',
    autor: '',
    categoria: '',
    editorial: '',
    nump: '',
    descripcion: '',
    imagenURL: ''
  };
}

const editarLibro = (lib) =>{
  nuevoLibro.value = lib;
  editado.value = true
}
const eliminarLibro = async (id) => {
  await axios.delete(`http://localhost:8088/libro/eliminar-libro/${id}`);
  console.log('Libro Eliminado con id:', id);
  await cargarLibro();
}

onMounted(cargarLibro);

</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12 mt-4">
        <div class="card shadow p-4 mb-4">
          <h2 class="text-center">Formulario de Libros</h2>
          <form @submit.prevent="agregarLibro">
            <div class="row">
            <div class="col-md-6 mb-3">
              <label for="titulo" class="form-label">Titulo</label>
              <input type="text" class="form-control" id="titulo" v-model="nuevoLibro.titulo" required>
            </div>
            <div class="col-md-6 mb-3">
              <label for="autor" class="form-label">Autor</label>
              <input type="text" class="form-control" id="autor" v-model="nuevoLibro.autor" required>
            </div>
            <div class="col-md-6 mb-3">
              <label for="categoria" class="form-label">Categoria</label>
              <input type="text" class="form-control" id="categoria" v-model="nuevoLibro.categoria" required>
            </div>
            <div class="col-md-6 mb-3">
              <label for="editorial" class="form-label">Editorial</label>
              <input type="text" class="form-control" id="editorial" v-model="nuevoLibro.editorial" required>
            </div>
            <div class="col-md-6 mb-3">
              <label for="nump" class="form-label">Numero de Paginas</label>
              <input type="number" class="form-control" id="nump" v-model="nuevoLibro.nump" required>
            </div>
            <div class="col-md-6 mb-3">
              <label for="descripcion" class="form-label">Descripcion</label>
              <input type="text" class="form-control" id="descripcion" v-model="nuevoLibro.descripcion" required>
            </div>
            <div class="col-md-6 mb-3">
              <label for="imagenURL" class="form-label">imagen URL</label>
              <input type="int" class="form-control" id="imagenURL" v-model="nuevoLibro.imagenURL" required>
            </div>
            </div>
            <button type="submit" class="btn btn-primary">
              {{ editado ? 'Actualizar Libro' : 'Agregar Libro' }}
            </button>
          </form>
        </div>
      </div>
      <div class="col-md-12">
        <div class="card shadow">
          <div class="card-body">
            <h5 class="card-title mb-3">Tabla de Libros</h5>
              <table class="table table-hover align-middle">
                <thead class="lable-light">
                  <tr>
                    <th scope="col">Id</th>
                    <th scope="col">Titulo</th>
                    <th scope="col">Autor</th>
                    <th scope="col">Categoria</th>
                    <th scope="col">Editorial</th>
                    <th scope="col">Num de Paginas</th>
                    <th scope="col">Descripcion</th>
                    <th scope="col">Imagen</th>
                  </tr>
                 </thead>
                 <tbody>
                  <tr v-for="libro in libros" :key="libro.id">
                    <td>{{ libro.id }}</td>
                    <td>{{ libro.titulo }}</td>
                    <td>{{ libro.autor }}</td>
                    <td>{{ libro.categoria }}</td>
                    <td>{{ libro.editorial }}</td>
                    <td>{{ libro.nump }}</td>
                    <td>{{ libro.descripcion }}</td>
                    <td><img :src="libro.imagenURL" alt="Imagen de Libro" width="50"></td>
                    <td>
                      <button @click="eliminarLibro(libro.id)" class="btn btn-danger mx-2"><i class="bi bi-trash2-fill"></i></button>
                      <button @click="editarLibro(libro)" class="btn btn-warning"><i class="bi bi-pencil-fill"></i></button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

      
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
