<template>
  <div class="container">
    <div class="right"></div>
    <div class="left">
      <div class="header">
        <h4 class="animation a1">
          ¡Por favor, complete el formulario para el Administrador!
        </h4>
        <br>
      </div>
      <div class="form">
        <input
          v-model="nombre"
          type="text"
          class="form-field animation a3"
          placeholder="Ingrese Nombre"
          required
        />
        <input
          v-model="apellido"
          type="text"
          class="form-field animation a3"
          placeholder="Ingrese Apellido"
        />
        <input
          v-model="correo"
          type="email"
          class="form-field animation a3"
          placeholder="example@yavirac.edu.ec"
          @input="validateEmail"
          required
        />
        <input
          v-model="celular1"
          type="text"
          class="form-field animation a3"
          placeholder="Ingrese Celular"
          @input="validateCelular"
          required
        />
        <input
          v-model="password"
          type="password"
          class="form-field animation a4"
          placeholder="Contraseña"
        />
        <button @click="register" class="animation a6">Registrate</button>
        <p class="animation a5">
          ¿Sí tengo una cuenta?
          <router-link to="/login"><strong>Inicia Sesión</strong></router-link>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";

export default {
  data() {
    return {
      nombre: "",
      apellido: "",
      correo: "",
      celular1: "",
      password: "",
      emailError: "",
      celularError: "",
    };
  },
  methods: {
    validateEmail() {
      // Regex to match email ending with @yavirac.edu.ec
      const emailPattern = /^[a-zA-Z0-9._%+-]+@yavirac\.edu\.ec$/;
      this.emailError = emailPattern.test(this.correo)
        ? ""
        : "El correo debe terminar en @yavirac.edu.ec";
    },
    validateCelular() {
      // Regex to match 10 digit numbers
      const celularPattern = /^\d{0,10}$/;
      this.celularError = celularPattern.test(this.celular1) && this.celular1.length === 10
        ? ""
        : "El celular debe contener solo números y tener exactamente 10 dígitos";
    },
    async register() {
      // Validate email and celular before proceeding
      this.validateEmail();
      this.validateCelular();

      if (this.emailError || this.celularError) {
        Swal.fire({
          icon: 'error',
          title: 'Error',
          text: this.emailError || this.celularError
        });
        return;
      }

      try {
        const response = await axios.post("http://localhost:3000/register", {
          nombre: this.nombre,
          apellido: this.apellido,
          correo: this.correo,
          celular1: this.celular1,
          password: this.password,
        });
        Swal.fire({
          icon: "success",
          title: "¡Registro exitoso!",
          text: "Redirigiendo al inicio de sesión...",
          timer: 2000,
          showConfirmButton: false,
        });
        setTimeout(() => {
          this.$router.push("/login"); // Redirige a la vista de inicio de sesión
        }, 2000);
      } catch (error) {
        const errorMessage =
          error.response?.data?.message ||
          "Ha ocurrido un error, por favor intenta nuevamente.";
        Swal.fire({
          icon: "error",
          title: "Error al registrarse",
          text: errorMessage,
        });
      }
    },
  },
};
</script>
<style scoped>

@import url("https://fonts.googleapis.com/css?family=Rubik:400,500&display=swap");



.container {
  display: flex;
  height: 100vh;
}

.left {
  align-items: center; /* Centrar horizontalmente */
  overflow: hidden;
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  justify-content: center;
  animation-name: left;
  animation-duration: 1s;
  animation-fill-mode: both;
  animation-delay: 1s;
}

.right {
  flex: 1;
  transition: 1s;
  background-image: url("/public/img/login.png");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}

.header > h4 {
  color: #4f46a5;
text-align: center;
}

.form {
  max-width: 80%;
  display: flex;
  flex-direction: column;
  text-align: center;
}

.form > p > a {
  color: #000;
  font-size: 15px;
  text-decoration: none;
}

.form-field {
  height: 30px;
  padding: 20px;
  border: 2px solid #656ed3;
  border-radius: 15px;
  outline: 0;
  transition: 0.2s;
  margin: 13px;
}

.form-field:focus {
  border-color: #0f7ef1;
}

.form > button {
  padding: 10px 10px;
  border: 0;
  background: linear-gradient(to right, #656ed3 0%, #afb3ff 100%);
  border-radius: 15px;
  margin: 5px;
  color: #fff;
  letter-spacing: 1px;
}

.form > button:hover {
  background: linear-gradient(to right, #4453fd 0%, #afb3ff 100%);
}

.animation {
  animation-name: move;
  animation-duration: 0.4s;
  animation-fill-mode: both;
  animation-delay: 2s;
}

.a1 {
  animation-delay: 2s;
}

.a3 {
  animation-delay: 2.2s;
}

.a4 {
  animation-delay: 2.3s;
}

.a5 {
  animation-delay: 2.4s;
}

.a6 {
  animation-delay: 2.5s;
}

@keyframes move {
  0% {
    opacity: 0;
    visibility: hidden;
    transform: translateY(-40px);
  }

  100% {
    opacity: 1;
    visibility: visible;
    transform: translateY(0);
  }
}

@keyframes left {
  0% {
    opacity: 0;
    width: 0;
  }

  100% {
    opacity: 1;
    padding: 20px 40px;
    width: 440px;
  }
}
</style>
