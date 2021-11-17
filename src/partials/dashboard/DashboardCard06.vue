<template>
  <div class="flex flex-col col-span-full sm:col-span-6 xl:col-span-4 bg-white shadow-lg rounded-sm border border-gray-200">
    <header class="px-5 py-4 border-b border-gray-100">
      <h2 class="font-semibold text-gray-800">Top Countries</h2>
    </header>
    <!-- Chart built with Chart.js 3 -->
    <!-- Change the height attribute to adjust the chart height -->
    <DoughnutChart :data="chartData" width="389" height="260" />
  </div>
</template>

<script>
import { ref } from 'vue'
import DoughnutChart from '../../charts/DoughnutChart.vue'
import EditMenu from '../../components/DropdownEditMenu.vue'

// Import utilities
import { tailwindConfig } from '../../utils/Utils'

export default {
  name: 'DashboardCard06',
  components: {
    DoughnutChart,
    EditMenu,
  },
  setup() {
    const chartData = ref({
      labels: ['United States', 'Italy', 'Other'],
      datasets: [
        {
          label: 'Top Countries',
          data: [
            35, 30, 35,
          ],
          backgroundColor: [
            tailwindConfig().theme.colors.indigo[500],
            tailwindConfig().theme.colors.blue[400],
            tailwindConfig().theme.colors.indigo[800],
          ],
          hoverBackgroundColor: [
            tailwindConfig().theme.colors.indigo[600],
            tailwindConfig().theme.colors.blue[500],
            tailwindConfig().theme.colors.indigo[900],
          ],
          hoverBorderColor: tailwindConfig().theme.colors.white,
        },
      ],
    })

    return {
      chartData,
    } 
  }
}
</script>