<template>
  <div>
    <ProtectedNavbar />
    <!-- Modal Crear Manager -->
    <top-bar /> <div :class="['content', theme]">
      <div class="content-wrapper" :class="theme">

      <div class="header">
        <div id="capa-padre">
          <div class="container text-center">
            <div class="row">
              <div class="col">
                <h1>Manager</h1>
              </div>
              <div class="col">
                <div id="app">
                  <button @click="showCreateModal = true">Agregar Manager</button>
                  <button
                    @click="toggleTheme"
                    class="theme-toggle"
                    style="padding-left: 10px; margin-left: 50px"
                    :class="theme"
                  >
                    <i class="bi bi-moon"></i>
                  </button>
                </div>
              </div>
            </div>
            <MyModal :isVisible="showCreateModal" @close="closeModals">
              <form @submit.prevent="handleCreate" :class="theme">
                <h2>Agregar Manager</h2>

                <h2>Crear Manager</h2>
                <!-- Subir Imagen -->
                <div class="form-group custom-form-group">
                  <label for="imagen" class="upload-label custom-upload-label">
                    <i class="bx bx-check"></i> Subir Imagen
                  </label>
                  <input
                    type="file"
                    id="imagen"
                    class="custom-upload-input"
                    @change="handleFileUpload"
                    accept="image/*"
                  />
                </div>
                <!-- Vista previa de la imagen seleccionada -->
                <div v-if="imagePreview" class="image-preview custom-image-preview">
                  <img
                    :src="imagePreview"
                    alt="Vista previa de la imagen"
                    class="custom-preview-img"
                  />
                </div>
                <div class="form-group">
                  <label for="nombre">Nombres:</label>
                  <input type="text" v-model="formData.firstName" required />
                </div>
                <div class="form-group">
                  <label for="apellido">Apellidos:</label>
                  <input type="text" v-model="formData.lastName" required />
                </div>
                <div class="form-group">
                  <label for="correo">Correo:</label>
                  <input type="email" v-model="formData.email" required />
                </div>
                <div class="form-group">
                  <label for="genero">Género:</label>
                  <input type="text" v-model="formData.gender" required />
                </div>
                <div class="form-group">
                  <label for="estado">Estado:</label>
                  <input
                    type="text"
                    v-model="formData.status"
                    required
                    disabled
                    style="background-color: #0000002a"
                  />
                </div>

                <div class="button-container">
                  <button type="submit">Guardar</button>
                </div>
              </form>
            </MyModal>

            <!-- Modal Editar Manager -->
            <MyModal :isVisible="showEditModal" @close="closeModals">
              <form @submit.prevent="handleEdit" :class="theme">
                <h2>Editar Manager</h2>
                <!-- Subir Imagen en edición -->
                <div class="form-group custom-form-group">
                  <label for="edit-imagen" class="upload-label custom-upload-label">
                    <i class="bx bx-check"></i> Cambiar Imagen
                  </label>
                  <input
                    type="file"
                    id="edit-imagen"
                    class="custom-upload-input"
                    @change="handleEditFileUpload"
                    accept="image/*"
                  />
                </div>
                <!-- Vista previa de la imagen seleccionada en edición -->
                <div v-if="editImagePreview" class="image-preview custom-image-preview">
                  <img
                    :src="editImagePreview"
                    alt="Vista previa de la imagen"
                    class="custom-preview-img"
                  />
                </div>
                <div class="form-group">
                  <label for="edit-nombre">Nombres:</label>
                  <input
                    type="text"
                    id="edit-nombre"
                    v-model="editFormData.firstName"
                    required
                  />
                </div>
                <div class="form-group">
                  <label for="edit-apellido">Apellidos:</label>
                  <input
                    type="text"
                    id="edit-apellido"
                    v-model="editFormData.lastName"
                    required
                  />
                </div>
                <div class="form-group">
                  <label for="edit-correo">Correo:</label>
                  <input
                    type="email"
                    id="edit-correo"
                    v-model="editFormData.email"
                    required
                  />
                </div>
                <div class="form-group">
                  <label for="edit-genero">Género:</label>
                  <input
                    type="text"
                    id="edit-genero"
                    v-model="editFormData.gender"
                    required
                  />
                </div>
                <div class="form-group">
                  <label for="estado">Estado:</label>
                  <input
                    type="text"
                    v-model="editFormData.status"
                    required
                    disabled
                    style="background-color: #0000002a"
                  />
                </div>

                <div class="button-container" >
                  <button type="submit" >Guardar Cambios</button>
                </div>
              </form>
            </MyModal>
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
      <router-link to="/maneger">Manager</router-link>
    </li>
  </ol>
</nav>
      <!-- Botones para exportar y buscar -->
      <div class="button-container">
        <button class="excel">EXCEL</button>
        <input
          type="text"
          placeholder="Buscar . . ."
          class="buscar"
          v-model="searchQuery"
        />
      </div>

      <!-- Tabla de Managers -->
      <div class="table-container">
        <table>
          <thead>
            <tr>
              <th :class="theme"><div class="cell">#</div></th>
              <th :class="theme"><div class="cell">Foto</div></th>
              <th :class="theme"><div class="cell">Apellidos</div></th>
              <th :class="theme"><div class="cell">Nombres</div></th>
              <th :class="theme"><div class="cell">Correo</div></th>
              <th :class="theme"><div class="cell">Género</div></th>
              <th :class="theme"><div class="cell">Estado</div></th>
              <th :class="theme"><div class="cell">Acciones</div></th>
            </tr>
          </thead>
          <tbody :class="theme">
            <tr v-for="(user, index) in filteredUsers" :key="index">
              <td>
                <div class="cell">{{ index + 1 }}</div>
              </td>
              <td>
                <div class="cell">
                  <img :src="user.photo" alt="" class="user-photo" />
                </div>
              </td>
              <td>
                <div class="cell">{{ user.lastName }}</div>
              </td>
              <td>
                <div class="cell">{{ user.firstName }}</div>
              </td>
              <td>
                <div class="cell">{{ user.email }}</div>
              </td>
              <td>
                <div class="cell">{{ user.gender }}</div>
              </td>
              <td>
                <span
                  :class="{
                    'status-active': user.status === 'Activo',
                    'status-inactive': user.status === 'Inactivo',
                  }"
                  >{{ user.status }}</span
                >
              </td>
              <td>
                <button class="btn view-btn" @click="viewUser(user)">
                  <i class="bx bx-show"></i>
                </button>
                <button class="btn edit-btn" @click="editUser(user)">
                  <i class="bx bx-edit"></i>
                </button>
                <button
                  class="btn delete-btn"
                  v-if="user.status === 'Activo'"
                  @click="deleteUser(user)"
                >
                  <i class="bx bx-trash"></i>
                </button>
                <button class="btn restore-btn" v-else @click="restoreUser(user)">
                  <i class="bx bx-undo"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import ProtectedNavbar from "../components/ProtectedNavbar.vue";
import MyModal from "../components/Modal.vue";
import Swal from "sweetalert2";
import { ref } from "vue";
import TopBar from "@/components/top-bar.vue";

export default {
  components: {
    ProtectedNavbar,
    MyModal,
    TopBar

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
      showCreateModal: false,
      showEditModal: false,
      searchQuery: "",
      formData: {
        firstName: "",
        lastName: "",
        email: "",
        gender: "",
        status: "Activo",
        photo: "path/to/default/photo.png", // Ejemplo de valor por defecto para la foto
      },
      editFormData: {}, // Para almacenar los datos del manager que se está editando
      users: [
        {
          firstName: "John",
          lastName: "Doe",
          email: "Alexander@ya.ed",
          gender: "Masculino",
          status: "Activo",
          photo:
            "https://wallpapers.com/images/featured/fotos-de-perfil-xj8jigxkai9jag4g.jpg",
        },
        {
          firstName: "Jane",
          lastName: "Smith",
          email: "Alexander@ya.ed",
          gender: "Femenino",
          status: "Inactivo",
          photo:
            "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSeiGQCoJHB8ls2xOC3gE8iEuF81qWCe_V9zA&s",
        },
        {
          firstName: "Alexander",
          lastName: "Narvaez",
          email: "Alexander@ya.ed",
          gender: "Masculino",
          status: "Activo",
          photo:
            "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTcNwi94JURMqWDKYEYQaobN4WaP4tNkx3oNQ&s",
        },
      ],
    };
  },
  computed: {
    filteredUsers() {
      const query = this.searchQuery.toLowerCase();
      return this.users.filter(
        (user) =>
          user.firstName.toLowerCase().includes(query) ||
          user.lastName.toLowerCase().includes(query) ||
          user.email.toLowerCase().includes(query) ||
          user.gender.toLowerCase().includes(query) ||
          user.status.toLowerCase().includes(query)
      );
    },
  },
  methods: {
    handleCreate() {
      const newUser = { ...this.formData };
      this.users.push(newUser);
      this.showCreateModal = false;
      this.resetFormData();
      Swal.fire({
        title: "¡Manager creado!",
        text: "El nuevo manager ha sido creado con éxito.",
        icon: "success",
        confirmButtonText: "OK",
      });
    },
    handleEdit() {
      // Encontrar el usuario en el array y actualizar sus datos
      const index = this.users.findIndex(
        (user) => user.email === this.editFormData.email
      );
      if (index !== -1) {
        this.users[index] = { ...this.editFormData };
        this.showEditModal = false;
        this.editFormData = {};
        Swal.fire({
          title: "¡Manager editado!",
          text: "Los cambios han sido guardados exitosamente.",
          icon: "success",
          confirmButtonText: "OK",
        });
      }
    },
    editUser(user) {
      // Mostrar el modal de edición y cargar los datos del usuario seleccionado
      this.editFormData = { ...user };
      this.showEditModal = true;
    },
    deleteUser(user) {
      // Cambiar el estado del usuario a "Inactivo"
      user.status = "Inactivo";
      Swal.fire({
        title: "¡Manager eliminado!",
        text: "El manager ha sido movido a la lista de Inactivos.",
        icon: "success",
        confirmButtonText: "OK",
      });
    },
    restoreUser(user) {
      // Restaurar el estado del usuario a "Activo"
      user.status = "Activo";
      Swal.fire({
        title: "¡Manager restaurado!",
        text: "El manager ha sido movido a la lista de Activos.",
        icon: "success",
        confirmButtonText: "OK",
      });
    },
    closeModals() {
      // Cerrar todos los modales y limpiar datos de edición
      this.showCreateModal = false;
      this.showEditModal = false;
      this.resetFormData();
      this.editFormData = {};
    },
    resetFormData() {
      // Reiniciar los datos del formulario de creación
      this.formData = {
        firstName: "",
        lastName: "",
        email: "",
        gender: "",
        status: "Activo",
        photo: "path/to/default/photo.png",
      };
    },
    viewUser(user) {
      Swal.fire({
        title: "Detalles del Manager",
        html: `
          <div class="modal-details">
            <p><strong>Nombres:</strong> ${user.firstName}</p>
            <p><strong>Apellidos:</strong> ${user.lastName}</p>
            <p><strong>Correo:</strong> ${user.email}</p>
            <p><strong>Género:</strong> ${user.gender}</p>
            <p><strong>Estado:</strong> ${user.status}</p>
            <img src="${user.photo}" alt="Foto del Manager" class="modal-photo">
          </div>
        `,
        confirmButtonText: "OK",
      });
    },
  },
};
</script>

<style scoped>
.content {
  min-height: 100vh;
  transition: background-color 0.3s ease, color 0.3s ease; /* Transición suave */
  display: flex;
  flex-direction: column; /* Organización en columna */
}
/* Estilo para el tema claro */
.light {
  background-color: #ffffff; /* Fondo blanco */
  color: #000000; /* Texto negro */
}

/* Estilo para el tema oscuro */
.dark {
  background-color: #555555; /* Fondo oscuro */
  color: #e0e0e0; /* Texto claro */
}
.content-wrapper {
  margin-top: 80px; /* Ajusta este valor según la altura de tu top-bar */
}
/* Estilos para el contenedor principal */
#capa-padre {
  background-image: url("/public/img/fondo 2.png");
  background-color: aliceblue;
  border-radius: 20px;
  text-align: center;
  margin: 5px auto;
  padding: 10px;
  box-shadow: 5px 2px 3px 1px rgba(0, 0, 0, 0.164);
  font-size: 16px;
  color: white;
}

/* Estilos para el formulario y botones */
#app {
  text-align: center;
  margin-top: 10px;
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

form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.form-group {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin-bottom: 15px;
  width: 70%;
}

.form-group label {
  flex: 1;
  margin-right: 10px;
  font-weight: bold;
  text-align: right;
}

.form-group input {
  flex: 2;
  padding: 5px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.button-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  background-color: aliceblue;
  padding: 10px;
  border-radius: 12px;
}

button[type="submit"] {
  padding: 10px 20px;
  font-size: 16px;
  color: white;
  background-color: #007bff;
  border: none;
  border-radius: 11px;
  cursor: pointer;
}

button[type="submit"]:hover {
  background-color: #0056b3;
}

/* Estilos para la tabla */
.table-container {
  padding: 20px;
  background-color: aliceblue;
  border-radius: 10px;
  box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.1);
  margin-top: 20px; /*con esto  puedo   bajar mas la tabla   para que haya espacio  entre el modal de crear y el cuadro */
}
table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 10px;
  text-align: center;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #f8f8f8;
  border-radius: 5px;
}

.cell {
  padding: 10px;
  border-radius: 5px;
  display: inline-block;
}

.user-photo {
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

.status-active {
  color: #fff;
  background-color: #28a745;
  padding: 5px 10px;
  border-radius: 15px;
}

.status-inactive {
  color: #fff;
  background-color: #dc3545;
  padding: 5px 10px;
  border-radius: 15px;
}

.btn {
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin: 0 2px;
}

.view-btn {
  background-color: #6c757d;
  color: #fff;
  box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.137);
}

.edit-btn {
  background-color: #ffc107;
  color: #fff;
  box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.137);
}

.delete-btn {
  background-color: #dc3545;
  color: #fff;
  box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.137);
}

.restore-btn {
  background-color: #17a2b8;
  color: #fff;
  box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.137);
}

.excel,
.buscar {
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin: 0 2px;
}

.excel {
  background-color: #28a745;
  color: #fff;
  margin-left: 10px;
  box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.137);
}

.buscar {
  background-color: #ffffff;
  color: black;
  box-shadow: 5px 2px 3px 1px rgba(0, 0, 0, 0.164);
  padding: 5px 10px;
  cursor: pointer;
  margin-left: 530px;
  width: 380px;
  border-radius: 10px;
  text-align: center;
}

.custom-form-group {
  display: flex;
  flex-direction: column;
}

.custom-upload-label {
  display: flex;
  align-items: center;
  background-color: #3c8dbc;
  color: white;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.custom-upload-input {
  display: none;
}

.custom-image-preview {
  margin-top: 10px;
}

.custom-preview-img {
  max-width: 80%;
  max-height: 50px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.song-photo {
  max-width: 70px;
  max-height: 50px;
  border-radius: 4px;
  object-fit: cover;
}

.buscar {
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

</style>
