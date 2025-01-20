<template>
  <div class="top-bar">
    <div class="left-section">
    </div>
    <div class="right-section">
     
      <div class="notification-icon-wrapper" @click="toggleModal" title="Abrir notificaciones">
        <i class="bx bx-bell notification-icon"></i>
        <!-- Indicador rojo -->
        <span v-if="notifications.length > 0" class="notification-badge">
          {{ notifications.length }}
        </span>
      </div>

      <div class="user-profile">
        <router-link to="/perfil" class="username">
          <span class="usuario-text">Usuario</span>
        </router-link>
        <router-link to="/perfil">
          <img
            src="https://i.pinimg.com/736x/48/22/bf/4822bf54ee339f9e3842192de95a906e.jpg"
            alt="Foto de perfil"
            class="profile-pic"
          />
        </router-link>
      </div>
    </div>

    <!-- Modal de Notificaciones -->
    <transition name="fade">
      <div v-if="showModal" class="notification-modal" @click.self="closeModal">
        <div class="modal-content">
          <h3>Notificaciones</h3>
          <p v-if="notifications.length > 0">
            Tienes {{ notifications.length }} notificación(es).
          </p>
          <p v-else>No tienes notificaciones.</p>
          <ul>
            <li
              v-for="notification in notifications"
              :key="notification.id"
              class="notification-item"
              @click="handleNotificationClick(notification.id)"
            >
              {{ notification.message }}
            </li>
          </ul>
          <button @click="closeModal">Cerrar</button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "TopBar",
  data() {
    return {
      showModal: false, // Controla la visibilidad del modal
      notifications: [
        { id: 1, message: "Evento ROck" },
        { id: 2, message: "Nueva notificación 2" },
        { id: 3, message: "Nueva notificación 3" },
      ], // Lista de notificaciones
    };
  },

  methods: {
    toggleModal() {
      this.showModal = !this.showModal; // Alternar el estado del modal
    },
    closeModal() {
      this.showModal = false; // Cerrar el modal
    },
    handleNotificationClick(id) {
      alert(`Notificación ${id} seleccionada`);
      // Aquí puedes agregar lógica para marcar como leída o redirigir.
    },
  },
};
</script>

<style scoped>




.top-bar {
  background-color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;

  padding: 10px 20px;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-bottom: 1px solid #ddd;
  position: fixed;
  top: 0;
  left: 250px;
  width: calc(100% - 250px);
  z-index: 1000;
}
.username {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-right: 20px ;
}
.left-section h2 {
  margin: 0;
  font-size: 20px;
  color: #333;
}

.right-section {
  display: flex;
  align-items: center;
}

.notification-icon-wrapper {
  position: relative;
  cursor: pointer;
}

.notification-icon {
  font-size: 24px;
  color: #555;
}

.notification-icon:hover {
  color: #0aa5a9;
}

.notification-badge {
  position: absolute;
  top: -5px;
  right: -5px;
  background-color: red;
  color: white;
  border-radius: 50%;
  font-size: 12px;
  font-weight: bold;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.2);
}

.user-profile {
  display: flex;
  align-items: center;
  margin-left: 20px;
}

.profile-pic {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  object-fit: cover;
  margin-right: 20px;
  border: 2px solid #0aa5a9;
}

/* Estilos del Modal */
.notification-modal {
  position: fixed;
  top: 50px;
  right: 20px;
  background-color: rgba(0, 0, 0, 0.3);
  width: 300px;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.1);
  z-index: 1001;
}

.modal-content {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
}

.modal-content h3 {
  margin: 0;
  font-size: 18px;
  color: #333;
}

.modal-content p {
  font-size: 16px;
  color: #555;
}

.notification-item {
  font-size: 14px;
  padding: 10px;
  margin-bottom: 5px;
  border-radius: 5px;
  background-color: #f5f5f5;
  cursor: pointer;
  transition: background-color 0.3s;
}

.notification-item:hover {
  background-color: #0aa5a9;
  color: white;
}

.modal-content button {
  margin-top: 10px;
  padding: 8px 15px;
  background-color: #0aa5a9;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.modal-content button:hover {
  background-color: #067b80;
}
</style>
