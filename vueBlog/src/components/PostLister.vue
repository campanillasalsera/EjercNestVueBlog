<script setup lang="ts">
import PostTemplate from './PostTemplate.vue'
import PostCreator from './PostCreator.vue'
import { Base64 } from 'js-base64';
import { onMounted, ref } from 'vue';

    //función para recuperar los datos del usuario desde la cookie
    //Debemos meterla en un store, pues está repetida en el App.vue
    let userData: any = undefined;
    function getUserData(): Object | undefined {
        let allCookiesOneString = document.cookie;
        let cookies = allCookiesOneString.split("; ");
        let userCookieEncoded = cookies.find((eachCookie) => {
            return eachCookie.split("=")[0] === "user"
        })
        const userCoookieValueEncoded = userCookieEncoded?.split("=")[1];
        const userCookieValue = userCoookieValueEncoded ? Base64.decode(userCoookieValueEncoded) : undefined;
        return userCookieValue ? JSON.parse(userCookieValue) : undefined;
    }
    userData = getUserData();


    //Para que typescript reconozca los tipos creamos esta interface de Post
    //con todos los tipos.
    interface Post {
    id: number; 
    titulo: string;
    contenido: string;
    owner: { email: string };
    createdAt: number; 
    updatedAt: number;
    }


    //para que cargue los posts una vez que el componente esté montado en el DOM
    onMounted(async () => {
    userData = getUserData();
    await mostrarPosts();
    });


    //función que realiza el fetch y devuelve la respuesta en json
    function getPosts() {
        return fetch('http://localhost:3000/posts')
        .then((response) => {
            if (!response.ok) {
                throw new Error('Error en la respuesta')
            }
            return response.json();
        })
        .catch((error) => {
            console.error("ERROR EN LA PETICIÓN:", error);
            return [];
        });
    }




 
//función que muestra los posts llamando a la función getPosts y guardando
//el resultado en la variable postRecibidos
    //creamos un array donde guardar las referencias
    let postsRecibidos = ref<Post[]>([]);
    async function mostrarPosts() {
        try {
            const posts = await getPosts();
            postsRecibidos.value = posts;
        } catch (error) {
            console.error("Error:", error);
        }
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
    <PostCreator v-if="userData && userData['isValid']"></PostCreator>
    <PostTemplate v-for="post in postsRecibidos"
        :key="post.id"
        class="post"
        :titulo="post.titulo"
        :contenido="post.contenido"
        :owner="post.owner.email"
        :createdAt="formatFechaISO8601(post.createdAt)"
        :updatedAt="formatFechaISO8601(post.updatedAt)">
    </PostTemplate>
</template>


<style>
    .post{
        margin: 30px;
    }
</style>