<template>
  <div class="col-span-full xl:col-span-6 bg-white shadow-lg rounded-sm border border-gray-200">
    <header class="px-5 py-4 border-b border-gray-100">
      <h2 class="font-semibold text-gray-800">Reason for Refunds</h2>
    </header>
    <div class="px-5 py-3">
      <div class="flex items-start">
        <div class="text-3xl font-bold text-gray-800 mr-2">449</div>
        <div class="text-sm font-semibold text-white px-1.5 bg-yellow-500 rounded-full">-22%</div>
      </div>
    </div>
    <!-- Chart built with Chart.js 3 -->
    <div class="grow">
      <!-- Change the height attribute to adjust the chart height -->
      <BarChart :data="chartData" :width="595" :height="48" />
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import BarChart from '../../charts/BarChart03.vue'

// Import utilities
import { tailwindConfig } from '../../utils/Utils'

export default {
  name: 'DashboardCard11',
  components: {
    BarChart,
  },  
  setup() {
    const chartData = ref({
      labels: ['Reasons'],
      datasets: [
        {
          label: 'Having difficulties using the product',
          data: [131],
          backgroundColor: tailwindConfig().theme.colors.indigo[500],
          hoverBackgroundColor: tailwindConfig().theme.colors.indigo[600],
          barPercentage: 1,
          categoryPercentage: 1,
        },
        {
          label: 'Missing features I need',
          data: [100],
          backgroundColor: tailwindConfig().theme.colors.indigo[800],
          hoverBackgroundColor: tailwindConfig().theme.colors.indigo[900],
          barPercentage: 1,
          categoryPercentage: 1,
        },
        {
          label: 'Not satisfied about the quality of the product',
          data: [81],
          backgroundColor: tailwindConfig().theme.colors['sky'][400],
          hoverBackgroundColor: tailwindConfig().theme.colors['sky'][500],
          barPercentage: 1,
          categoryPercentage: 1,
        },
        {
          label: 'The product doesn’t look as advertised',
          data: [65],
          backgroundColor: tailwindConfig().theme.colors.green[400],
          hoverBackgroundColor: tailwindConfig().theme.colors.green[500],
          barPercentage: 1,
          categoryPercentage: 1,
        },
        {
          label: 'Other',
          data: [72],
          backgroundColor: tailwindConfig().theme.colors.gray[200],
          hoverBackgroundColor: tailwindConfig().theme.colors.gray[300],
          barPercentage: 1,
          categoryPercentage: 1,
        },
      ],
    })

    return {
      chartData,
    } 
  }
}
</script>