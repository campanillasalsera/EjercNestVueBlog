<script setup lang="ts">
import { ref } from 'vue';


const emit = defineEmits(['goToPost']);


const props = defineProps<{
  postData: {
  id: number
  titulo: string,
  contenido: string,
  usuario: string,
  createdAt: string,
  updatedAt: string
},
shouldGetData: boolean
}>()

let postDataFromServer = ref(props.postData)
const shouldGetData = ref(props.shouldGetData);

if(shouldGetData.value){
    alert("llamando a la api")

    fetch('http://localhost:3000/post/', {
      method: "GET",
      headers: {
          "Content-Type": "application/json"
      },
      credentials: 'include',
    })
    .then(async (response) => {
      const data = await response.json() as {
        id: number,
        titulo: string,
        contenido: string,
        usuario: string,
        createdAt: string,
        updatedAt: string
      } 

      postDataFromServer.value = data
      console.log("data: "+data)
    })

}


    //función que formatea la fecha que me devuelve la base de datos en el 
//formato que se le indica
function formatFechaISO8601(fechaISO: string | number | Date) {
    const date = new Date(fechaISO);
    const dia = date.getUTCDate();
    const mes = date.getUTCMonth() + 1; // Los meses en JavaScript son de 0 a 11
    const año = date.getUTCFullYear();

    return `${dia}-${mes}-${año}`;
}
</script>

<template>
    <article class="articulo">
     <h1 @click="emit('goToPost', postDataFromServer.id)" class="green pointer">{{ postDataFromServer.titulo }}</h1>
     <h5 class="subtitle">Por {{ postDataFromServer.usuario }}</h5>
     <h5 class="subtitle">Publicado el  {{ formatFechaISO8601(postDataFromServer.createdAt) }}</h5>
     <h5 class="subtitle">Actualizado el  {{ formatFechaISO8601(postDataFromServer.updatedAt) }}</h5>
     <hr>
     <p>{{ postDataFromServer.contenido }}</p>
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

.pointer{
    cursor: pointer;
}
.subtitle{
    color: rgb(195, 202, 209);
}

hr{
    border: 1px solid grey;
}
</style>