<script setup lang="ts">
import PostTemplate from './PostTemplate.vue'
import PostCreator from './PostCreator.vue'
import { Base64 } from 'js-base64';
import { useRouter } from 'vue-router';
import { ref, type Ref } from 'vue';


const router = useRouter()

    /**función para recuperar los datos del usuario desde la cookie
    Debemos meterla en un store, pues está repetida en el App.vue
    
     */
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


    let postsRef: Ref<{
    id: number,
    titulo: string,
    contenido: string,
    usuario: string,
    createdAt: string,
    updatedAt: string
    }[]> = ref([]);
    



    //función que realiza el fetch y devuelve la respuesta en json
    function getpostsData() {
 fetch('http://localhost:3000/posts', {
    method: 'GET',
    headers: { "Content-Type": "application/json"},
    credentials: 'include',
  })
  .then(async (response) => {
    const data = await response.json()
    postsRef.value = data;
  })
  .catch((error) => {
      console.error("ERROR EN LA PETICIÓN:", error);
  });
}

getpostsData();


//Función que abre una nueva ventana al hacer click en un post
function goToPost(id:number) {
    router.push({path: `/post/${id}`})
}

</script>


<template>
    <PostCreator 
        @postCreated="getUserData" 
        v-if="userData && userData['isValid']">
    </PostCreator>
    <PostTemplate @go-to-post="goToPost" :should-get-data="false" 
      :post-data="post" :key="post.id" v-for="post of postsRef"></PostTemplate>

</template>


<style>
    .post{
        margin: 30px;
    }
</style>