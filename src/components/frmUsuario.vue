<script setup lang="ts">
import { ref, watch, reactive } from 'vue'

/* tambien funciona !!
interface Usuario {
  nombre: string, 
  email: string, 
  password: string, 
  fnacimiento: string,
  edad: number
}

const usuario_ = ref<Usuario>({nombre: "", email: "", password: "", fnacimiento: "", edad: 0});
*/

const usuario_ = reactive({nombre: "", email: "", password: "", fnacimiento: "", edad: 0});

const onSubmit = () => {
  if (usuario_.nombre && usuario_.password && usuario_.email && usuario_.fnacimiento)
    alert('Enviar datos ingresados');
  else
    alert('Atencion !! . Faltan completar dato(s) a enviar');
}

const fnLimpiar = () => {
  //alert('Limpiar datos ingresados');
  usuario_.nombre     = "";
  usuario_.email      = "";
  usuario_.password   = "";
  usuario_.fnacimiento= "";
  //usuario_.edad= 0;
}

watch( () => usuario_, (newValue, oldValue) => {
    //console.log('with deep', newValue.fnacimiento, oldValue.fnacimiento);
    const fhoy = new Date();
    const fnac = new Date(newValue.fnacimiento);
    let edad = fhoy.getFullYear() - fnac.getFullYear();
    //console.log('edad', edad, fhoy, fnac);
    const mes = fhoy.getMonth() - fnac.getMonth();
    if (mes < 0 || (mes === 0 && fhoy.getDate() < fnac.getDate())) {
        edad--;
    }
    //console.log('datos', usuario_.edad, edad, mes);
    newValue.edad = edad;  //o usuario_.edad = edad.toString();    
  },
  { deep: true }
)

/*
watch(usuario_, (newValue, oldValue) => {
  console.log(`watch: count changed from ${oldValue} to ${newValue}`);
});
*/

/*
watch: {
  'usuario_.fnacimiento': function (newValue, oldValue) {
    const hoy = new Date();
    const nacimiento = new Date(newValue);
    usuario_.edad = hoy.getFullYear() - nacimiento.getFullYear();
    const mes = hoy.getMonth() - nacimiento.getMonth();
    if (mes < 0 || (mes === 0 && hoy.getDate() < nacimiento.getDate())) {
        usuario_.edad--;
    }
  }
}

function fnCalcularEdad(fnacimiento: string): number {
    const hoy = new Date();
    const nacimiento = new Date(fnacimiento);
    let edad = hoy.getFullYear() - nacimiento.getFullYear();
    const mes = hoy.getMonth() - nacimiento.getMonth();
    if (mes < 0 || (mes === 0 && hoy.getDate() < nacimiento.getDate())) {
        edad--;
    return edad;
  }
}
*/

</script>

<template>
  <h1>{{ msg }}</h1>

  <form @submit.prevent="onSubmit">
    <fieldset>
    <legend >Datos solicitados</legend>
    <label for="nombre">Nombre:</label><br>
    <input type="text" id="nombre" v-model.trim="usuario_.nombre" size="50" placeholder="Ingrese nombre(s) y apellido(s)"><br>
    <label for="email">Email:</label><br>
    <input type="email" id="email" v-model.trim="usuario_.email" size="50" placeholder="Ingrese una direccion de correo valida"><br>
    <label for="password">Contraseña:</label><br>
    <input type="password" id="password" v-model.trim="usuario_.password" size="15" placeholder="Ingrese una contraseña segura"><br>
    <label for="fnacimiento">Fecha nacimiento:</label><br>
    <input type="date" id="fnacimiento" size="20" v-model="usuario_.fnacimiento"><br><br>
    </fieldset> 

    <br>

    <button type="submit">Enviar</button>
    <input type="reset" value="Limpiar datos" @click="fnLimpiar">

    <br><br>

  </form>

  <div>
    <h5>Por favor, verifique los datos ingresados antes de enviar</h5>
    <p><span>Nombre:</span> {{ usuario_.nombre }}</p>
    <p><span>Email:</span> {{ usuario_.email }}</p>
    <p><span>Password:</span> {{ usuario_.password }}</p>
    <p><span>Nacimiento:</span> {{ usuario_.fnacimiento }}</p>
    <p><span>Edad:</span> {{ usuario_.edad }}</p>
  </div>

  <!-- 
  <div>
  <table>
   <caption>Datos solicitados a enviar</caption>
   <thead>
   <tr>
       <th>Campo</th>
       <th>Valor</th>                        
   </tr>
   </thead>
   <tbody>
   <tr>
       <td>Nombre</td>
       <td ${{ usuario_.nombre }}></td>
   </tr>
   <tr>
       <td>Email</td>
       <td {{ usuario_.email }}></td>
   </tr>
   <tr>
       <td>Fecha nacimiento</td>
       <td {{ usuario_.fnacimiento }}></td>
   </tr>
   <tr>
       <td>Password</td>
       <td {{ usuario_.password }}></td>
   </tr>
   <tr>
       <td>Edad</td>
       <td {{ usuario_.edad }}></td>
   </tr>                
   </tbody>
  </table>
  </div>
  -->
</template>

<style scoped>
.read-the-docs {
  color: #888;
}

label {
    margin: 20px;    
}

p {
    width: 400px;
    padding: 5px;
    margin: 5px;
    border: 3px solid #ccc;
    border-radius: 5px;
    text-align: left;
}

span {
    display: inline-block;
    width: 80px;
    margin-right: 5px;
}

input[type="text"], input[type="email"], input[type="password"], input[type="date"] {
    width: 300px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

button {
    padding: 10px 20px;
    margin-left: 15px;
    background-color: #4CAF50;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

input[type="reset"] {
    padding: 10px 20px;
    margin-left: 15px;
    background-color: #4CAF50;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}
</style>
