<script setup>
import { ref, onMounted, nextTick, watch } from 'vue';
import { Chart, LineController, LineElement, PointElement, LinearScale, Title, CategoryScale, Tooltip } from 'chart.js';

Chart.register(LineController, LineElement, PointElement, LinearScale, Title, CategoryScale, Tooltip);

const props = defineProps({
  labels: {
    type: Array,
    required: true,
  },
  data: {
    type: Array,
    required: true,
  },
});

const chartRef = ref(null);
let chartInstance = null;

async function setupChart() {
  await nextTick();
  if (!chartRef.value) return;

  if (chartInstance) {
    chartInstance.destroy();
  }

  const ctx = chartRef.value.getContext('2d');

  const gradient = ctx.createLinearGradient(0, 0, 0, 300);
  gradient.addColorStop(0, 'rgba(66, 165, 245, 1)');
  gradient.addColorStop(1, 'rgba(66, 165, 245, 0)');

  chartInstance = new Chart(ctx, {
    type: 'line',
    data: {
      labels: props.labels,
      datasets: [
        {
          label: 'Temperature (°C)',
          data: props.data,
          borderColor: '#42A5F5',
          backgroundColor: gradient,
          fill: true,
          pointBackgroundColor: '#ffffff',
          pointBorderColor: '#42A5F5',
          pointRadius: 5,
          tension: 0.4,
        },
      ],
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          display: true,
          position: 'top',
          labels: {
            color: '#ffffff',
            font: {
              size: 14,
            },
          },
        },
        tooltip: {
          backgroundColor: 'rgba(66, 165, 245, 0.8)',
          titleFont: { size: 16 },
          bodyFont: { size: 14 },
          borderWidth: 1,
          borderColor: '#ffffff',
        },
      },
      scales: {
        x: {
          title: {
            display: true,
            text: 'Time',
            color: '#ffffff',
            font: { size: 14 },
          },
          ticks: {
            color: '#ffffff',
            font: { size: 12 },
          },
          grid: {
            color: 'rgba(255, 255, 255, 0.2)',
          },
        },
        y: {
          title: {
            display: true,
            text: 'Temperature (°C)',
            color: '#ffffff',
            font: { size: 14 },
          },
          ticks: {
            color: '#ffffff',
            font: { size: 12 },
          },
          grid: {
            color: 'rgba(255, 255, 255, 0.2)',
          },
        },
      },
    },
  });
}

watch(() => props.data, setupChart);

onMounted(setupChart);
</script>

<template>
  <canvas ref="chartRef" style="max-width: 100%; height: 300px;"></canvas>
</template>

<style scoped>
canvas {
  display: block;
  width: 100%;
  background: rgba(0, 0, 0, 0.5);
  border-radius: var(--border-radius);
  padding: 10px;
}
</style>
