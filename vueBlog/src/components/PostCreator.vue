<script setup lang="ts">
import { ref } from 'vue';


    function crearPost() {
        fetch('http://localhost:3000/post', {
            method: "POST",
            body: JSON.stringify({
                titulo: tituloRef.value,
                contenido: contenidoRef.value
            }),
            headers: {"Content-Type": "application/json"},
            credentials: 'include'
        })
        .then(() => { alert("Post creado") })
        .catch((e) => {
            console.error(e);
            alert("Ha ocurrido un error")
        })
    }


    let tituloRef = ref();
    let contenidoRef = ref()

</script>

<template>
    <div class="fromCreateContainer">
        <h3 class="green">Crea un nuevo post</h3>
        <hr class="saltoLinea">
        <form @submit.prevent="crearPost" class="formCreateContainer">
            <label for="titulo" class="green">Título</label>
            <input v-model="tituloRef" type="text" name="titulo" id="titulo">
            <label for="contenido" class="green">Contenido</label>
            <textarea v-model="contenidoRef" name="contenido" id="contenido" cols="30" rows="10"></textarea>
            <button type="submit">Crear Post</button>
        </form>
    </div>
</template>

<style>
.formCreateContainer{
    display: flex;
    flex-direction: column;
    justify-content: center;margin-top: 20px;
}
.formCreateContainer input, .formCreateContainer textarea{
    background-color: rgb(39, 38, 38);
    color: aliceblue;
}
</style>