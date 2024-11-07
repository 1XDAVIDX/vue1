<script>
import Swal from 'sweetalert2';
import axios from 'axios';
import { ref } from 'vue';
import { useRouter } from 'vue-router';

export default {
    setup() {
        const verificar = ref({
            idCompra: 0,
            idUsuario: ""
        });
        const factura = ref({});
        
        const pedidoHecho = async () => {
            try {
                // Cambiar la URL a la del servidor Spring Boot
                const respuesta = await axios.delete(`http://localhost:8080/compra/compra/completada/${verificar.value.idCompra}/${verificar.value.idUsuario}`);
                
                // Asignar la respuesta a la variable factura
                factura.value = respuesta.data;
                
                // Mostrar la alerta con los detalles de la compra
                Swal.fire({
                    icon: "success",
                    title: "Pedido Completado",
                    text: `Producto: ${factura.value.idProducto} Cantidad: ${factura.value.cantidad} Total: ${factura.value.total}\nID compra: ${factura.value.idCompra} ID Usuario: ${factura.value.idUsuario} Nombre: ${factura.value.nombre} Correo: ${factura.value.email}`
                });
            } catch (error) {
                // Si ocurre un error, mostrar la alerta de error
                Swal.fire({
                    icon: "error",
                    title: "Error",
                    text: "Datos no son correctos o ocurrió un problema con la eliminación de la compra"
                });
            }
        };
        
        const router = useRouter();
        
        return {
            verificar,
            pedidoHecho,
            factura,
            router
        };
    }
};
</script>

<template>
    <form @submit.prevent="pedidoHecho" class="formulario">
        <div id="tituloheader">
            <h1 id="titulo">Proceso Pedido</h1>
            <button type="button" @click="router.go(-1)" id="x">X</button>
        </div>
        <label class="respuesta">ID Compra:
            <input v-model="verificar.idCompra" type="number" required>
        </label>
        <label class="respuesta">ID Usuario:
            <input v-model="verificar.idUsuario" type="text" required>
        </label>
        <button type="submit" class="boton">Completado</button>
    </form>
</template>

<style>
.formulario {
    width: 100%;
    max-width: 400px; /* Ancho máximo */
    margin: 40px auto; /* Centrado vertical */
    padding: 20px; /* Espaciado interno */
    border-radius: 10px; /* Bordes redondeados */
    background-color: #f9f9f9; /* Color de fondo */
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1); /* Sombra */
}

#tituloheader {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

#titulo {
    font-size: 24px; /* Tamaño del texto */
    color: #333; /* Color del texto */
}

#x {
    background: none; 
    border: none; 
    font-size: 20px; 
    color: #ff5555; 
    cursor: pointer; 
}

.respuesta {
    display: flex;
    flex-direction: column;
    margin-top: 20px;
    width: 100%;
}

.respuesta label {
    margin-bottom: 10px; /* Espaciado entre las etiquetas */
    font-weight: bold; /* Negrita para las etiquetas */
    color: #555; /* Color de las etiquetas */
}

.respuesta input {
    padding: 10px; /* Espaciado interno */
    border: 1px solid #ccc; /* Borde del input */
    border-radius: 5px; /* Bordes redondeados */
    transition: border 0.3s; /* Transición suave para el borde */
}

.respuesta input:focus {
    border-color: #007BFF; /* Color del borde al enfocar */
    outline: none; /* Sin contorno */
}

.boton {
    margin-top: 30px;
    padding: 10px; /* Espaciado interno */
    background-color: #007BFF; /* Color de fondo */
    color: white; /* Color del texto */
    border: none; /* Sin borde */
    border-radius: 5px; /* Bordes redondeados */
    cursor: pointer; /* Cursor pointer para indicar interactividad */
    transition: background-color 0.3s; /* Transición suave para el fondo */
}

.boton:hover {
    background-color: #0056b3; /* Color de fondo al pasar el mouse */
}
</style>
