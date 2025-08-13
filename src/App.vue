<script setup>
import { ref, onMounted, watch } from 'vue'
import { db } from './data/libros' 
import Libro from './components/Libro.vue' 
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

const libros = ref([])
const carrito = ref([])
const libro = ref({})

watch(carrito, () => {
  guardarLocalStorage()
}, {
  deep: true
})

const guardarLocalStorage = () => {
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

onMounted(() => {
  libros.value = db
  libro.value = db[0] 

  const carritoGuardado = localStorage.getItem('carrito')
  if (carritoGuardado) {
    carrito.value = JSON.parse(carritoGuardado)
  }
})

const vaciarCarrito = () => {
  carrito.value = []
  localStorage.removeItem('carrito')
  alert('Carrito de libros vaciado')
}

const agregarCarrito = (libro) => {
  const existeCarrito = carrito.value.findIndex(producto => 
    producto.id === libro.id
  )
  
  if (existeCarrito >= 0) {
    carrito.value[existeCarrito].cantidad++
  } else {
    libro.cantidad = 1
    carrito.value.push(libro)
  }
  guardarLocalStorage()
}

const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  if (carrito.value[index].cantidad <= 1) return 
  carrito.value[index].cantidad--
}

const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  carrito.value[index].cantidad++
}
</script>

<template>
  <!-- Renderización del Header -->
  <Header 
    :carrito="carrito"
    :libro="libro"
    @decrementar-cantidad="decrementarCantidad"
    @incrementar-cantidad="incrementarCantidad"
    @agregar-carrito="agregarCarrito" 
    @vaciar-carrito="vaciarCarrito"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Catálogo de Libros</h2>
    <div class="row mt-5">
      <!-- Renderización de Libros -->
      <Libro 
        v-for="libro in libros"
        :key="libro.id"
        :libro="libro" 
        @agregar-carrito="agregarCarrito"
      />
    </div>
  </main>

  <!-- Renderización del Footer -->
  <Footer/>
</template>

<style scoped>
h1 {
  text-transform: uppercase;
  color: green;
}
</style>
