<template>
  <div class="container">
    <div class="left">
      <div class="header">
        <h1 class="animation a1">Bienvenido</h1>
        <br>
      </div>
      <div class="form">
        <input
          v-model="correo"
          type="email"
          class="form-field animation a3"
          placeholder="example@yavirac.edu.ec"
          @input="validateEmail"
          required
        />
        <input
          v-model="password"
          type="password"
          class="form-field animation a4"
          placeholder="Contraseña"
        />
        <button @click="login" class="animation a6">Ingresar</button>
        <p class="animation a5">
          Si no tienes cuenta?<router-link to="/registro"
            ><strong> Registrate </strong></router-link
          >
        </p>
      </div>
    </div>
    <div class="right"></div>
  </div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  data() {
    return {
      correo: "",
      password: "",
      emailError: "",
      // Datos quemados para autenticación
      dummyUser: {
        correo: "admin@yavirac.edu.ec",
        password: "admin123",
      },
    };
  },
  methods: {
    validateEmail() {
      const emailPattern = /^[a-zA-Z0-9._%+-]+@yavirac\.edu\.ec$/;
      this.emailError = emailPattern.test(this.correo)
        ? ""
        : "El correo debe terminar en @yavirac.edu.ec";
    },
    login() {
      this.validateEmail();

      if (this.emailError) {
        Swal.fire({
          icon: "error",
          title: "Error",
          text: this.emailError,
        });
        return;
      }

      // Verificación con datos quemados
      if (
        this.correo === this.dummyUser.correo &&
        this.password === this.dummyUser.password
      ) {
        Swal.fire({
          icon: "success",
          title: "¡Ingreso exitoso!",
          text: "Redirigiendo al dashboard...",
          timer: 2000,
          showConfirmButton: false,
        });
        setTimeout(() => {
          localStorage.setItem("token", "dummy-token"); // Simula guardar un token
          this.$router.push("/dashboard");
        }, 2000);
      } else {
        Swal.fire({
          icon: "error",
          title: "Error al iniciar sesión",
          text: "Correo o contraseña incorrectos. Intenta nuevamente.",
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
  align-items: center;
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
  background-image: url("/public/img/registro.png");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}

.header > h1 {
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
