<template>
  <div class="flex flex-col col-span-full sm:col-span-6 bg-white shadow-lg rounded-sm border border-gray-200">
    <header class="px-5 py-4 border-b border-gray-100">
      <h2 class="font-semibold text-gray-800">Direct VS Indirect</h2>
    </header>
    <!-- Chart built with Chart.js 3 -->
    <!-- Change the height attribute to adjust the chart height -->
    <BarChart :data="chartData" width="595" height="248" />
  </div>
</template>

<script>
import { ref } from 'vue'
import BarChart from '../../charts/BarChart01.vue'

// Import utilities
import { tailwindConfig } from '../../utils/Utils'

export default {
  name: 'DashboardCard03',
  components: {
    BarChart,
  },
  setup() {
    const chartData = ref({
      labels: [
        '12-01-2020', '01-01-2021', '02-01-2021',
        '03-01-2021', '04-01-2021', '05-01-2021',
      ],
      datasets: [
        // Light blue bars
        {
          label: 'Direct',
          data: [
            800, 1600, 900, 1300, 1950, 1700,
          ],
          backgroundColor: tailwindConfig().theme.colors.blue[400],
          hoverBackgroundColor: tailwindConfig().theme.colors.blue[500],
          barPercentage: 0.66,
          categoryPercentage: 0.66,
        },
        // Blue bars
        {
          label: 'Indirect',
          data: [
            4900, 2600, 5350, 4800, 5200, 4800,
          ],
          backgroundColor: tailwindConfig().theme.colors.indigo[500],
          hoverBackgroundColor: tailwindConfig().theme.colors.indigo[600],
          barPercentage: 0.66,
          categoryPercentage: 0.66,
        },
      ],
    })

    return {
      chartData,
    }    
  }
}
</script>