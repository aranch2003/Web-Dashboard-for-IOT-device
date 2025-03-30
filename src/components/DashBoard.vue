<template>
    <div class="dashboard">
      <h1>Sensor Data Dashboard</h1>
      <div class="charts">
        <SensorChart :sensorData="temperatureData" label="Temperature" />
        <SensorChart :sensorData="humidityData" label="Humidity" />
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted, onUnmounted } from 'vue';
  import SensorChart from './SensorChart.vue';
  
  export default {
    name: 'DashBoard',
    components: { SensorChart },
    setup() {
      const temperatureData = ref([]);
      const humidityData = ref([]);
      let dataInterval = null;
  
      // ✅ Fix: Use a new array reference instead of modifying the existing one
      const generateData = (dataRef) => {
        const newData = [...dataRef.value]; // Create a shallow copy
        if (newData.length >= 20) newData.shift();
        newData.push(parseFloat((Math.random() * 100).toFixed(2)));
        dataRef.value = newData; // Replacing the reference to trigger Vue reactivity
      };
  
      onMounted(() => {
        dataInterval = setInterval(() => {
          generateData(temperatureData);
          generateData(humidityData);
        }, 1000);
      });
  
      onUnmounted(() => {
        clearInterval(dataInterval);
      });
  
      return { temperatureData, humidityData };
    },
  };
  </script>
  
  <style scoped>
  .dashboard {
    padding: 20px;
    text-align: center;
    background-color: #1e1e1e;
    color: #ffffff;
  }
  
  .charts {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
    flex-wrap: wrap;
  }
  
  h1 {
    font-size: 2rem;
    margin-bottom: 20px;
  }
  </style>
  