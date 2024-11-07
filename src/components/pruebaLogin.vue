<script>
import Swal from 'sweetalert2';
import { useRouter } from 'vue-router';
import axios from 'axios';
import { ref } from 'vue';

export default {
    setup() {

        function crearCuenta() {
            router.push({name:"registroUsuario"})
        }
        const login = ref({
            idUsuario:"",
            contraseña: "",
            
        });
        const message = ref('');
        const mirar = ref({});
        
        
                const insertarlogin = async () => {
            if (!login.value.idUsuario || !login.value.contraseña) {
                Swal.fire({
                    icon: 'error',
                    title: 'Campos vacíos',
                    text: 'Por favor, completa todos los campos'
                });
                return;
            }

            try {
                const respuesta = await axios.post('http://localhost:8080/usuario/login', null, {
                    params: {
                        id_usuario: login.value.idUsuario,
                        contraseña: login.value.contraseña
                    }
                });
                

                message.value = respuesta.data.message;
                mirar.value = respuesta.data.rol;
                
                Swal.fire({
                    icon: 'success',
                    title: 'Inicio de sesión exitoso',
                    text: 'Bienvenido a tu cuenta'
                });
                console.log("rol:", message.value)
                if (mirar.value === "admin") {
                    router.replace({ name: "admin" });
                } else if (mirar.value === "cliente") {
                    router.replace({ name: "cliente" });
                }
            } catch (error) {
                console.error("Error al registrar datos", error);
                Swal.fire({
                    icon: 'error',
                    title: 'Error de inicio de sesión',
                    text: 'Usuario No Existe'
                });
            }
        };
        const router = useRouter();
        
        return {
            login,
            insertarlogin,
            router,
            crearCuenta
        };
    }
};
</script>

<template>
    <form @submit.prevent="insertarlogin" class="formulario">
        <div id="tituloheader">
        <h1 id="titulo">INICIO SESIÓN</h1>
        <button type="button" @click="()=>router.go(-1)"  id="x">X</button>
        </div>
        <label class="respuesta">ID usuario:
            <input v-model="login.idUsuario" type="text" required>
        </label>
        
        <label class="respuesta">Contraseña:
            <input v-model="login.contraseña" type="password" required>
        </label>
        
        <button type="submit" class="boton">Registrar</button>
        <button @click="crearCuenta"  class="boton">No estoy registrado</button>
    </form>
    
</template>

<style>
#x {
    background: none; 
    border: none; 
    font-size: 20px; 
    color: #ff5555; 
    cursor: pointer; 
}

.formulario {
    
    position: fixed; 
    top: 10%; 
    width: 20%;
    left: 50%; 
    transform: translateX(-50%); 
    background-color: white;
    padding: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); 
    z-index: 1000; 


}

.respuesta{
    display: flex;
    flex-direction: column;
    margin-top: 20px;
    width: 100%;
}
.boton{
    margin-top: 30px;
    
    display: block;
    width: 100%; 
    box-sizing: border-box;
    
}
#tituloheader{
    display: flex;
    
}

#x{
    background-image: url('./img/x.png');
    background-size: contain;
    margin-left: auto;
    width: 40px;
    height: 40px;
    border-radius: 50%;

}
.mensaje{
    text-align: center;
    margin-top: 20px;
    background-color: seagreen;
    color:  white;
    width: 20%;
    margin-left: auto;
    margin-right: auto;
}

</style>