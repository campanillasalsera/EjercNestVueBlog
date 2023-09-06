<script setup lang="ts">
import { ref } from 'vue';
import { useRouter } from 'vue-router';

// let emailRegex = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
let router = useRouter();

function registerUser() {

    if (passRef.value !== confirmPassRef.value){
        alert("Las contraseñas no coinciden");
    }else{
        fetch('http://localhost:3000/user', {
            method: "POST",
            body: JSON.stringify({
                email: emailRef.value,
                pass: passRef.value
            }),
            headers: {
                "Content-Type": "application/json"
            }
        }).then((response) => {
            if (response.ok) {
                alert("Usuario registrado");
                emailRef.value = ""; // Reiniciar el campo de email
                passRef.value = ""; 
                confirmPassRef.value = ""; 
                router.push('/posts');
            } else if (response.status === 403) {
                alert("El email "+ emailRef.value+ " ya está registrado");
                emailRef.value = ""; // Reiniciar el campo de email
            } else {
                console.log("Error desconocido");
            }
        }).catch((e) => {
            console.error(e)
            alert("Ha ocurrido un error");
        });
    }
        
    }

    // Referencias para enviar los valores de los inputs
    let emailRef = ref("");
    let passRef = ref("");
    let confirmPassRef = ref("");



</script>

<template>
  <div class="registerform">
    <h1 class="green">"ESTO ES EL FORMULARIO"</h1>
    <form @submit.prevent="registerUser">
        <label for="emailInput" class="green">Email:</label>
        <input type="email" id="emailInput" v-model="emailRef" pattern="[a-zA-Z0-9!#$%&'*\/=?^_`\{\|\}~\+\-]([\.]?[a-zA-Z0-9!#$%&'*\/=?^_`\{\|\}~\+\-])+@[a-zA-Z0-9]([^@&%$\/\(\)=?¿!\.,:;]|\d)+[a-zA-Z0-9][\.][a-zA-Z]{2,4}([\.][a-zA-Z]{2})?" title="Email no válido">
        <label for="passInput" class="green">Contraseña:</label>
        <input type="password" id="passInput" v-model="passRef" >
        <label for="passInputRepeat" class="green"> Repite Contraseña:</label>
        <input type="password" id="passInputRepeat" v-model="confirmPassRef" >
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