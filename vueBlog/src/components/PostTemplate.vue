<script setup lang="ts">
import { ref } from 'vue';


const props = defineProps({
    titulo: String,
    contenido: String,
    owner: String,
    createdAt: String, 
    updatedAt: String
})

</script>

<template>
    <article class="articulo">
     <h1 class="green">{{ titulo }}</h1>
     <h5 class="subtitle">Por {{ owner }}</h5>
     <h5 class="subtitle">Publicado el  {{ createdAt }}</h5>
     <h5 class="subtitle">Actualizado el  {{ updatedAt }}</h5>
     <hr>
     <p>{{ contenido }}</p>
    </article> 
 </template>

<style>
.articulo{
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.articulo p{
    text-align: justify;
}

.subtitle{
    color: rgb(195, 202, 209);
}

hr{
    border: 1px solid grey;
}
</style>