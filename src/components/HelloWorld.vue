<script setup>
defineProps({
  msg: String,
})


</script>
<template>
  <div class="container">
    <h1>{{ msg }}</h1>
    <div >
      <label>Mensaje:</label>
      <input type="text" v-model="mensaje"  />
    </div>
    <div >
      <label>Clave:</label>
      <input type="text" v-model="clave" />
    </div>
    <br>
    <button class="button1" @click="cifrar">Cifrar</button>
    <button class="button2" @click="descifrar">Descifrar</button>

    <div class="output-group">
      <label>Texto Cifrado:</label><br>
      <textarea v-model="mensajeCifrado" ></textarea>
    </div>

    <div class="output-group">
      <label>Mensaje Descifrado:</label><br>
      <textarea v-model="mensajeDescifrado" ></textarea>
    </div>
  </div>
</template>

<script>

import sjcl from 'sjcl';

export default {
  data() {
    return {
      msg: 'Cifrado simetrico con metodo SJCL',
      mensaje: '',
      clave: '',
      mensajeCifrado: '',
      mensajeDescifrado: '',
    };
  },
  methods: {
    cifrar() {
      if (this.mensaje && this.clave) {
        this.mensajeCifrado = this.encryptSJCL(this.mensaje, this.clave);
      }
    },
    descifrar() {
      if (this.mensajeCifrado && this.clave) {
        this.mensajeDescifrado = this.decryptSJCL(this.mensajeCifrado, this.clave);
      }
    },
    encryptSJCL(data, key) {
      const encryptedData = sjcl.encrypt(key, data);
      return btoa(encryptedData);
    },
    decryptSJCL(encryptedData, key) {
  
      const decodedData = atob(encryptedData);
      const decryptedData = sjcl.decrypt(key, decodedData);
      return decryptedData;
    },
  },
};
</script>

<style scoped>
.container {
  text-align: center;
  margin: 20px;
}

input{
  width: 300px;
  height: 30px;
  margin: 10px;
}

.button1 {
  width: 100px;
  height: 40px;
  margin: 10px;
  background-color: rgb(10, 168, 23);
}

.button2{
  width: 100px;
  height: 40px;
  margin: 10px;
  background-color: rgb(71, 126, 247);
}
textarea {
  width: 400px;
  height: 100px;
}
</style>