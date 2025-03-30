<template>
    <div class="chart-container">
      <canvas ref="sensorChart"></canvas>
    </div>
  </template>
  
  <script>
  import { onMounted, watch, ref } from 'vue';
  import Chart from 'chart.js/auto';
  
  export default {
    name: 'SensorChart',
    props: {
      sensorData: Array,
      label: String,
    },
    setup(props) {
      const sensorChart = ref(null);
      let chartInstance = null;
  
      const createChart = () => {
        if (!sensorChart.value) return;
        
        chartInstance = new Chart(sensorChart.value, {
          type: 'line',
          data: {
            labels: props.sensorData.map((_, index) => `Point ${index + 1}`),
            datasets: [{
              label: props.label,
              data: props.sensorData,
              borderColor: 'rgba(75, 192, 192, 1)',
              borderWidth: 2,
              fill: false,
            }],
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
              legend: {
                display: true,
                labels: { color: '#ffffff' }, // White legend for dark theme
              },
            },
            scales: {
              x: { ticks: { color: '#ffffff' } },
              y: { ticks: { color: '#ffffff' } },
            },
          },
        });
      };
  
      const updateChart = () => {
        if (chartInstance) {
          chartInstance.data.labels = props.sensorData.map((_, index) => `Point ${index + 1}`);
          chartInstance.data.datasets[0].data = props.sensorData;
          chartInstance.update();
        }
      };
  
      onMounted(() => {
        createChart();
      });
  
      watch(() => props.sensorData, (newData, oldData) => {
        if (!newData || newData.length === 0 || JSON.stringify(newData) === JSON.stringify(oldData)) return;
        updateChart();
      }, { deep: true });
  
      return { sensorChart };
    },
  };
  </script>
  
  <style scoped>
  .chart-container {
    width: 100%; /* Make the chart take full width in its container */
    max-width: 600px;
    height: 300px;
  }
  </style>
  