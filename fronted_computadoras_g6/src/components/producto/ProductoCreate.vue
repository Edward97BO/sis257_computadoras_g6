<script setup lang="ts">
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'
import type { Categoria } from '@/models/categoria';

var categorias = ref<Categoria[]>([])
async function getCategorias() {
  categorias.value = await http.get('categorias').then((response) => response.data)
}

onMounted(() => {
  getCategorias()
})

const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const nombre = ref('')
const descripcion = ref('')
const precio = ref('')
const stock = ref('')
const url = ref('')
const idCategoria = ref('')

async function crearProducto() {
  await http
    .post(ENDPOINT, {
      nombre: nombre.value,
      descripcion: descripcion.value,
      precio: precio.value,
      stock: stock.value,
      url: url.value,
      idCategoria: idCategoria.value
    })
    .then(() => router.push('/productos'))
}

function goBack() {
  router.go(-1)
}
</script>

<template>
  <div class="container">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item">
          <RouterLink to="/productos">Productos</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page">Crear</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Crear Nuevo Producto</h2>
    </div>

    <div class="row">
      <form @submit.prevent="crearProducto">
        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="nombre" placeholder="Nombre" pattern="[A-Za-záéíóúÁÉÍÓÚñÑ\s]+" title="No se Puede Ingresar Números" required />
          <label for="nombre">Nombre</label>
        </div>
        <div class="form-floating">
          <input
            type="text"
            class="form-control"
            v-model="descripcion"
            placeholder="Descripcion"
            required
          />
          <label for="descripcion">Descripción</label>
        </div>
        <div class="form-floating">
          <input
            type="number"
            step="0.01"
            class="form-control"
            v-model="precio"
            placeholder="Precio"
            required
          />
          <label for="precio">Precio</label>
        </div>
        <div class="form-floating">
          <input type="number" class="form-control" v-model="stock" placeholder="Stock" required />
          <label for="stock">Stock</label>
        </div>
        <div class="form-floating">
          <input type="text" class="form-control" v-model="url" placeholder="Url" required />
          <label for="url">Url Imagen</label>
        </div>
        <div class="form-floating mb-3">
          <select v-model="idCategoria" class="form-select">
            <option v-for="categoria in categorias" :value="categoria.id" :key="categoria.id">
              {{ categoria.nombre }}
            </option>
          </select>
          <label for="categoria">Categoría</label>
        </div>
        <div class="text-center mt-3">
          <button type="submit" class="btn btn-primary btn-lg">
            <font-awesome-icon icon="fa-solid fa-floppy-disk" />
          </button>
        </div>
      </form>
    </div>
    <div class="text-left">
      <button class="btn btn-link" @click="goBack">Volver</button>
    </div>
  </div>
</template>

<style></style>
