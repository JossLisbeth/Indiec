<template>
  <div class="profile-page">
    <ProtectedNavbar />
    <top-bar />
    <div :class="['content', theme]">
      <div class="profile-header">
        <div class="container">
          <img src="https://para-pc.net/wp-content/uploads/2021/10/12709.jpg" alt="Banner" class="banner-image" />
        </div>
        <div class="profile-info">
          <img :src="profile.photo" alt="Perfil" class="profile-image floating" />
          <div class="profile-details">
            <h1 class="profile-name">{{ profile.name }}</h1>
            <p>{{ profile.profession }}</p>
            <button @click="editProfile" class="edit-profile-btn">Editar perfil</button>
            <button @click="toggleTheme" class="theme-toggle" :class="theme" style="padding-left: 10px;margin-left: 50px;"> <i class="bi bi-moon"></i></button>
          </div>
        </div>
      </div>
 <!-- Migajas de pan -->
 <nav aria-label="breadcrumb">
  <ol class="breadcrumb">
    
    <li class="breadcrumb-item">
      <router-link to="/dashboard">Incio</router-link>
    </li>
    <li class="breadcrumb-item">
      <router-link to="/musica">Perfil</router-link>
    </li>
  </ol>
</nav>
      <nav class="profile-nav">
        <ul>
          <li class="active">Acerca de</li>
          <li>Publicaciones</li>
        </ul>
      </nav>

      <div class="profile-content">
        <h2>Información Personal</h2>
        <div class="profile-details-grid">
          <div class="detail-item"><span>Email:</span> {{ profile.email }}</div>
          <div class="detail-item"><span>Teléfono:</span> {{ profile.phone }}</div>
          <div class="detail-item"><span>Ubicación:</span> {{ profile.location }}</div>
        </div>
      </div>

      <MyModal :isVisible="showEditModal" @close="closeModal">
        <form @submit.prevent="updateProfile" class="edit-profile-form">
          <h2 class="modal-title">Editar Perfil</h2>
          <div class="form-group">
            <div class="form-group">
              <label>Foto de Perfil:</label>
              <input type="file" @change="handlePhotoChange" class="form-input" />
            </div>

            <label>Nombre:</label>
            <input type="text" v-model="formData.name" class="form-input" required />
          </div>
          <div class="form-group">
            <label>Email:</label>
            <input type="email" v-model="formData.email" class="form-input" required />
          </div>
          <div class="form-group">
            <label>Teléfono:</label>
            <input type="text" v-model="formData.phone" class="form-input" required />
          </div>
          <div class="form-group">
            <label>Ubicación:</label>
            <input type="text" v-model="formData.location" class="form-input" required />
          </div>
          <div class="button-container">
            <button type="submit" class="submit-button">Guardar Cambios</button>
          </div>
        </form>
      </MyModal>
    </div>
  </div>
</template>

<script>
import ProtectedNavbar from "../components/ProtectedNavbar.vue";
import MyModal from "../components/Modal.vue";
import Swal from "sweetalert2";
import TopBar from "@/components/top-bar.vue";
import { ref } from "vue";


export default {
  components: {
    ProtectedNavbar,
    MyModal,
    TopBar,
  },
  setup() {
    // Variable que controla el tema
    const theme = ref(localStorage.getItem("theme") || "light");

    // Método para alternar entre claro y oscuro
    const toggleTheme = () => {
      theme.value = theme.value === "light" ? "dark" : "light";
      localStorage.setItem("theme", theme.value); // Guardar en localStorage
    };

    return { theme, toggleTheme };
  },
  data() {
    return {
      showEditModal: false,
      profile: {
        photo: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSFTRmKWNwnE3VoZh6ZIZqAglV1oqUvGFlSUA&s",
        name: "John Doe",
        profession: "Artista del momento",
        email: "john.doe@example.com",
        phone: "123-456-7890",
        location: "Ciudad, País",
      },
      formData: {},
    };
  },
  methods: {
    editProfile() {
      this.formData = { ...this.profile };
      this.showEditModal = true;
    },
    updateProfile() {
      this.profile = { ...this.formData };
      this.showEditModal = false;
      Swal.fire("¡Éxito!", "El perfil ha sido actualizado exitosamente.", "success");
    },
    closeModal() {
      this.showEditModal = false;
    },
    handlePhotoChange(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.profile.photo = e.target.result;  // Actualizamos la foto de perfil
        };
        reader.readAsDataURL(file); // Leemos el archivo como una URL
      }
    },
  },
};
</script>

<style scoped>
/* Animación suave para el movimiento de la imagen */
@keyframes floating {
  0% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-10px);
  }

  100% {
    transform: translateY(0);
  }
}

.floating {
  animation: floating 3s infinite ease-in-out;
}

.profile-page {
  font-family: 'Arial', sans-serif;
  background: #f4f4f9;
  color: #333;
}

.profile-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #d2dbdc;
  padding: 10px 0;
}

.container {
  width: 700px;
  height: 200px;
}

/* Estilo para el tema claro */
.light {
  background-color: #ffffff; /* Fondo blanco */
  color: #000000; /* Texto negro */
}

/* Estilo para el tema oscuro */
.dark {
  background-color: #555555; /* Fondo oscuro */
  color: #000000; /* Texto claro */
}

button {
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
  background-color: #0aa5a9;
  color: white;
  border-radius: 13px;
  text-transform: capitalize;
}

button:hover {
  background-color: #067b80;
}

.banner-image {
  width: 800px;
  height: 250px;
  border-radius: 10px;
  /* Centra la imagen */
  background-repeat: no-repeat;
  /* Evita que la imagen se repita */
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  /* Sombra suave para darle más profundidad */
  
}

.profile-info {
  display: flex;
  align-items: center;
  margin-top: -50px;
  animation: slideUp 1s ease-out;
}

.profile-image {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  border: 4px solid #fff;
  margin-right: 20px;
}


.profile-details {
  color: #fff;
}

.profile-name {
  font-size: 2rem;
  font-weight: bold;
  color: #fff;
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
  animation: bounceIn 2s ease-out;
}

.edit-profile-btn {
  background: #43a397;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}


.profile-nav ul {
  display: flex;
  justify-content: center;
  list-style: none;
  padding: 10px 0;
  background: #fff;
  margin: 0;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
}

.profile-nav li {
  margin: 0 15px;
  cursor: pointer;
  padding: 5px 10px;
  transition: background-color 0.3s;
}

.profile-nav li:hover {
  background: #f4f4f9;
}

.profile-nav li.active {
  border-bottom: 2px solid #067b80;
  font-weight: bold;
}

.profile-content {
  background: #fff;
  margin: 20px auto;
  padding: 20px;
  border-radius: 10px;
  max-width: 800px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  animation: fadeIn 1.5s ease-out;
  margin-left: 180px;
}

.profile-details-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  animation: scaleIn 1s ease-out;
}

.detail-item {
  padding: 15px;
  background: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 5px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.detail-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
}

/* Modal */
.edit-profile-form {
  background: #f4f4f9;
  border-radius: 12px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  padding: 25px;
  width: 100%;
  max-width: 450px;
  margin: 0 auto;
  animation: scaleIn 0.3s ease-in-out;
}

.modal-title {
  font-size: 1.8rem;
  margin-bottom: 20px;
  color: #067b80;
  text-align: center;
  font-family: "Arial", sans-serif;
  text-transform: uppercase;
  letter-spacing: 2px;
  border-bottom: 2px solid #067b80;
  padding-bottom: 10px;
}

.form-group {
  margin-bottom: 20px;
  
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 600;
  color: #333;
  font-size: 0.95rem;
}

.form-input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1rem;
  color: #333;
  background: #fff;
  transition: border-color 0.3s, box-shadow 0.3s;
}



.button-container {
  text-align: center;
}

.submit-button {
  background: #43a397;
  color: #fff;
  padding: 12px 30px;
  font-size: 1rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s, transform 0.2s;
}

.breadcrumb {
  display: flex;
  flex-wrap: wrap;
  padding: 10px 15px;
  list-style: none;
  background-color: #f8f9fa;
  border-radius: 5px;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.1);
  align-items: center;
  justify-content: center;

}

.breadcrumb-item {
  font-size: 16px;
  font-weight: 500;
  color: #007bff;
}

.breadcrumb-item a {
  text-decoration: none;
  color: #0aa5a9;
  padding: 5px 8px;
  border-radius: 5px;
  transition: all 0.3s ease;
}

.breadcrumb-item a:hover {
  background-color: #067b80;
  color: #ffffff;
}



/* Animaciones */
@keyframes fadeIn {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

@keyframes slideUp {
  from {
    transform: translateY(20px);
    opacity: 0;
  }

  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes bounceIn {
  0% {
    transform: translateY(20px);
    opacity: 0;
  }

  60% {
    transform: translateY(-10px);
    opacity: 1;
  }

  100% {
    transform: translateY(0);
  }
}

@keyframes scaleIn {
  from {
    transform: scale(0.9);
    opacity: 0;
  }

  to {
    transform: scale(1);
    opacity: 1;
  }
}
</style>