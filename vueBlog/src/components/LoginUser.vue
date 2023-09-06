<script setup lang="ts">
    import { ref } from 'vue';
    import { useRouter } from 'vue-router';

    let router = useRouter();

    function loginUser() {
        
        fetch('http://localhost:3000/login', {
                method: "POST",
                body: JSON.stringify({
                    email: emailRef.value,
                    pass: passRef.value
                }),
                headers: {
                    "Content-Type": "application/json"
                },
                credentials: 'include'
            }).then((response) => {
                if (response.status === 204) {
                    alert("Usuario logueado")
                    //redireccion
                    router.push('/posts')
                    
                } else if (response.status === 403){
                    alert("Acceso denegado")
                    emailRef.value = "";
                    passRef.value = "";
                }
            }).catch((e) => {
                console.error(e)
                alert("Ha ocurrido un error");
            });
    }

    let emailRef = ref("");
    let passRef = ref("");

</script>

<template>
    <div class="registerform">
      <h1 class="green">"LOGIN"</h1>
      <form @submit.prevent="loginUser">
          <label for="emailInput" class="green">Email:</label>
          <input type="email" id="emailInput" v-model="emailRef" pattern="[a-zA-Z0-9!#$%&'*\/=?^_`\{\|\}~\+\-]([\.]?[a-zA-Z0-9!#$%&'*\/=?^_`\{\|\}~\+\-])+@[a-zA-Z0-9]([^@&%$\/\(\)=?¿!\.,:;]|\d)+[a-zA-Z0-9][\.][a-zA-Z]{2,4}([\.][a-zA-Z]{2})?" title="Email no válido">
          <label for="passInput" class="green">Contraseña:</label>
          <input type="password" id="passInput" v-model="passRef" >
          <button type="submit">Enviar</button>
      </form>
    </div>
  </template>

<style>
.registerform{
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.registerform form {
    display: flex;
    flex-direction: column;
    justify-content: center;
}
.registerform form input{
    height: 25px;
    border-radius: 8px;
}
.registerform form button{
    margin-top: 5px;
    background-color: hsla(160, 100%, 37%, 1);
    color: white;
    border-radius: 8px;
    height: 25px;
}
</style>

