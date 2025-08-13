<script setup>
    const props = defineProps({
        libro: {
            type: Object,
            required: true
        }
    })

    defineEmits(['agregar-carrito'])
</script>

<template>
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">
            <img 
                class="img-fluid" 
                v-bind:src="'/img/' + libro.imagen + '.jpg'" 
                :alt="'imagen libro ' + libro.nombre">
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase">{{ libro.nombre }}</h3>
           
            <p>{{ libro.descripcion }}</p>
            <p class="fw-bold">Editorial: {{ libro.editorial }}</p>
            <p class="fw-bold">Autor: {{ libro.autor }}</p>
            <p class="fw-black text-primary fs-3">Lps.{{ libro.precio }}</p>
            <button 
                type="button"
                class="btn btn-dark w-100"
                @click="$emit('agregar-carrito', libro)"
            >Agregar al Carrito</button>
        </div>
    </div><!-- FIN LIBRO -->
</template>

<style scoped>

</style>
