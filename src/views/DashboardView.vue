<template>
  <ProtectedNavbar />
  <top-bar />
  <div class="content">
    <div class="content-wrapper">
      <div class="header">
        <div id="inicio">
          <h1 style="color: white">
            "Da vida a la música: organiza géneros, crea grupos, impulsa artistas y lleva
            tus eventos al siguiente nivel."
          </h1>
        </div>
        <div id="capa-padre">
          <div>
            <div class="chart-container">
              <!-- Gráfico Circular (al lado izquierdo) -->
              <div class="chart-item">
                <div class="chart-box">
                  <h2 class="chart-title">MUSICA</h2>
                  <canvas id="pieChart" width="200" height="200"></canvas>
                </div>
              </div>
              <!-- Calendario (centrado y ensanchado) -->
              <div class="chart-item calendar-container">
                <!-- Sección del DatePicker -->
                <div>
                  <Datepicker
                    v-model="selectedDate"
                    :format="'yyyy-MM-dd'"
                    :inline="true"
                    :hide-input="true"
                  />
                </div>
              </div>
              <!-- Notificaciones (al lado derecho del calendario) -->
              <div class="notifications-container">
                <div
                  v-for="(notification, index) in notifications"
                  :key="index"
                  class="notification-bar"
                >
                  <p>{{ notification.date }}</p>
                  <!-- Barra de Progreso Estática y de color verde -->
                  <progress class="green-progress" value="100" max="100"></progress>
                </div>
              </div>
            </div>
            <div class="cards-container">
              <div
                v-for="(data, index) in percentageData"
                :key="data.id"
                class="card"
                @mouseover="hoveredIndex = index"
                @mouseleave="hoveredIndex = null"
              >
                <div
                  class="card-image"
                  :style="{
                    backgroundImage: `url(${
                      hoveredIndex === index ? data.hoverImage : data.image
                    })`,
                  }"
                ></div>
                <div class="main-content">
                  <p>{{ data.name }}</p>
                  <div class="rating-container">
                    <div class="coin-base">
                      <img
                        src="https://i.postimg.cc/T1F1K0bW/Ethereum.png"
                        alt="Ethereum"
                        class="small-image"
                      />
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- Añadido nuevo DatePicker -->
          <div>
            <DatePicker />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProtectedNavbar from "../components/ProtectedNavbar.vue";
import { ref, onMounted } from "vue";
import Datepicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";
import Chart from "chart.js/auto";
import TopBar from "@/components/top-bar.vue";

export default {
  name: "Dashboard",
  components: {
    ProtectedNavbar,
    Datepicker,
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

    const percentageData = ref([
      {
        id: 1,
        name: "Ballenita",
        image: "https://i.postimg.cc/NfR2yhNs/image-equilibrium.jpg",
        hoverImage: "https://i.postimg.cc/7Yk5szS7/hover-image-1.jpg",
      },
      {
        id: 2,
        name: "Rumbera",
        image: "https://i.postimg.cc/NfR2yhNs/image-equilibrium.jpg",
        hoverImage: "https://i.postimg.cc/7Yk5szS7/hover-image-2.jpg",
      },
      {
        id: 3,
        name: "sacudelo",
        image: "https://i.postimg.cc/NfR2yhNs/image-equilibrium.jpg",
        hoverImage: "https://i.postimg.cc/7Yk5szS7/hover-image-3.jpg",
      },
      {
        id: 4,
        name: "El Gusanito",
        image: "https://i.postimg.cc/NfR2yhNs/image-equilibrium.jpg",
        hoverImage: "https://i.postimg.cc/7Yk5szS7/hover-image-4.jpg",
      },
      {
        id: 5,
        name: "La Rumba",
        image: "https://i.postimg.cc/NfR2yhNs/image-equilibrium.jpg",
        hoverImage: "https://i.postimg.cc/7Yk5szS7/hover-image-5.jpg",
      },
    ]);

    const selectedDate = ref(null);

    const pieChartData = ref({
      labels: ["ballenita", "Rumbera", "sacudelo", "El Gusanito", "La Rumba"],
      datasets: [
        {
          data: [10, 20, 60, 15, 25],
          backgroundColor: ["#FF8977", "#89A2EB", "#FFCE89", "#FFCE19", "#36A2EB"],
        },
      ],
    });

    const pieChart = ref(null);

    const notifications = ref([
      { date: "Fecha del evento: 11-01-2025", message: "¡Evento en 5 días!" },
      { date: "Fecha del evento: 12-01-2025", message: "¡Próximo evento en 2 días!" },
      {
        date: "Fecha del evento: 15-01-2025",
        message: "¡Última oportunidad para registrarse!",
      },
    ]);

    const createPieChart = () => {
      const pieCtx = document.getElementById("pieChart")?.getContext("2d");
      if (pieCtx) {
        if (pieChart.value) {
          pieChart.value.destroy();
        }
        pieChart.value = new Chart(pieCtx, {
          type: "pie",
          data: pieChartData.value,
          options: {
            responsive: true,
            plugins: {
              legend: {
                position: "top",
              },
            },
          },
        });
      } else {
        console.error("No se pudo obtener el contexto del canvas para pieChart.");
      }
    };

    onMounted(() => {
      createPieChart();
    });

    const hoveredIndex = ref(null);

    return {
      theme,
      toggleTheme,
      percentageData,
      selectedDate,
      notifications,
      hoveredIndex,
    };
  },
};
</script>

<style scoped>
/*com*/
.content-wrapper {
  margin-top: 80px; /* Ajusta este valor según la altura de tu top-bar */
}

@import "../../node_modules/@syncfusion/ej2-base/styles/material.css";
@import "../../node_modules/@syncfusion/ej2-buttons/styles/material.css";
@import "../../node_modules/@syncfusion/ej2-vue-calendars/styles/material.css";
@import "@vuepic/vue-datepicker/dist/main.css";

#inicio h1 {
  font-size: 1.5em;
  font-weight: normal;
}

#inicio {
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url("/public/img/fondo 2.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  text-align: center;
  padding: 5px;
  border-radius: 20px;
}

#capa-padre {
  background-color: aliceblue;
  border-radius: 20px;
  text-align: center;
  margin: 5px auto;
  padding: 10px;
  box-shadow: 5px 2px 3px 1px rgba(0, 0, 0, 0.164);
  font-size: 16px;
  color: black;
  overflow: hidden;
}

.chart-container {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

.chart-item {
  width: auto;
}

.chart-container {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  gap: 20px;
  align-items: center;
  width: 100%;
  text-align: center;
}

.calendar-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 10px;
  width: auto;
  padding: 10px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin: 0 auto;
}

.notifications-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
  align-items: flex-start;
}

.notification-bar {
  width: 250px;
  padding: 10px;
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.chart-box {
  border: 2px solid #ccc;
  padding: 15px;
  border-radius: 8px;
  text-align: center;
  width: 300px;
  height: 300px;
}

canvas {
  width: 240px !important;
  height: 230px !important;
  max-width: none !important;
  max-height: none !important;
}

.chart-title {
  font-size: 1.2em;
  margin-bottom: 10px;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  margin-top: 50px;
}

.card {
  width: 1000px;
  height: 250px;
  background-color: #00033c;
  border-radius: 15px;
  margin-bottom: 1rem;
  padding: 0.5rem;
  box-sizing: border-box;
  overflow: hidden;
  text-align: center;
}

.card-image {
  width: 100%;
  height: 150px;
  background-size: cover;
  background-position: center;
  border-radius: 8px;
  transition: background-image 0.3s ease;
}

.main-content {
  text-align: center;
  color: #ddd;
}

.rating-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.5em;
  font-size: 0.95em;
}

.coin-base {
  display: flex;
  justify-content: center;
  align-items: center;
}

.small-image {
  width: 1em;
  margin-right: 0.5em;
}

/* Barra de progreso verde y estática */
.green-progress {
  width: 100%;
  height: 20px;
  background-color: #e0e0e0;
  border-radius: 10px;
  -webkit-appearance: none;
  appearance: none;
}

.green-progress::-webkit-progress-bar {
  background-color: #e0e0e0;
  border-radius: 10px;
}

.green-progress::-webkit-progress-value {
  background-color: green;
  border-radius: 10px;
}

.green-progress::-moz-progress-bar {
  background-color: green;
  border-radius: 10px;
}

/* Responsive */
@media (min-width: 600px) {
  .card {
    max-width: 140px;
  }
}

@media (min-width: 768px) {
  .card {
    max-width: 160px;
  }
}

@media (min-width: 992px) {
  .card {
    max-width: 180px;
  }
}

@media (min-width: 1200px) {
  .cards-container {
    justify-content: space-around;
  }
}
</style>
