<script>
import Swal from 'sweetalert2';
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
  setup() {
    const verificar = ref({
      producto: { idProducto: "" },  // Estructura para Producto
      usuario: { idUsuario: "" },   // Estructura para Usuario
      cantidad: 0
    });

    const comprar = async () => {
      try {
        // Enviar el objeto de compra correctamente estructurado
        const respuesta = await axios.post("http://localhost:8080/compra/realizar", {
          producto: {
            idProducto: verificar.value.producto.idProducto
          },
          usuario: {
            idUsuario: verificar.value.usuario.idUsuario
          },
          cantidad: verificar.value.cantidad
        });

        Swal.fire({
          icon: "success",
          title: "Compra Exitosa"
        });

      } catch (error) {
        Swal.fire({
          icon: "error",
          text: "Error al comprar: " + error.response.data
        });
      }
    };

    const router = useRouter();
    return {
      verificar,
      comprar,
      router
    };
  }
};
</script>

<template>
  <form @submit.prevent="comprar" class="formulario">
    <div id="tituloheader">
      <h1 id="titulo">Comprar</h1>
      <button type="button" @click="router.go(-1)" id="x">X</button>
    </div>
    <label class="respuesta">ID Producto:
      <input v-model="verificar.producto.idProducto" type="text" required>
    </label>
    <label class="respuesta">ID Usuario:
      <input v-model="verificar.usuario.idUsuario" type="text" required>
    </label>
    <label class="respuesta">Cantidad:
      <input v-model="verificar.cantidad" type="number" required>
    </label>
    <button type="submit" class="boton">Registrar</button>
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
  width: 20%;
  margin-left: auto;
  margin-right: auto;
}

.respuesta {
  display: flex;
  flex-direction: column;
  margin-top: 20px;
  width: 100%;
}

.boton {
  margin-top: 30px;
  display: block;
  width: 100%;
  box-sizing: border-box;
}
</style>
