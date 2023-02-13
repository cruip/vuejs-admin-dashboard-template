<template>
  <div class="px-5 py-3">
    <div class="flex items-start">
      <div class="text-3xl font-bold text-gray-800 mr-2 tabular-nums">$<span ref="chartValue">57.81</span></div>
      <div ref="chartDeviation" class="text-sm font-semibold text-white px-1.5 rounded-full"></div>
    </div>
  </div>
  <div class="grow">
    <canvas ref="canvas" :data="data" :width="width" :height="height"></canvas>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, watch } from 'vue'
import {
  Chart, LineController, LineElement, PointElement, LinearScale, TimeScale, Tooltip,
} from 'chart.js'
import 'chartjs-adapter-moment'

// Import utilities
import { tailwindConfig, formatValue } from '../utils/Utils'

Chart.register(LineController, LineElement, PointElement, LinearScale, TimeScale, Tooltip)

export default {
  name: 'RealtimeChart',
  props: ['data', 'width', 'height'],
  setup(props) {

    const canvas = ref(null)
    const chartValue = ref(null)
    const chartDeviation = ref(null)
    let chart = null

    // function that updates header values
    const handleHeaderValues = (data, chartValue, chartDeviation) => {
      const currentValue = data.datasets[0].data[data.datasets[0].data.length - 1]
      const previousValue = data.datasets[0].data[data.datasets[0].data.length - 2]
      const diff = ((currentValue - previousValue) / previousValue) * 100
      chartValue.value.innerHTML = data.datasets[0].data[data.datasets[0].data.length - 1]
      if (diff < 0) {
        chartDeviation.value.style.backgroundColor = tailwindConfig().theme.colors.yellow[500]
      } else {
        chartDeviation.value.style.backgroundColor = tailwindConfig().theme.colors.green[500]
      }
      chartDeviation.value.innerHTML = `${diff > 0 ? '+' : ''}${diff.toFixed(2)}%`
    }    
    
    onMounted(() => {
      const ctx = canvas.value
      chart = new Chart(ctx, {
        type: 'line',
        data: props.data,
        options: {
          layout: {
            padding: 20,
          },
          scales: {
            y: {
              border: {
                display: false,
              },
              suggestedMin: 30,
              suggestedMax: 80,
              ticks: {
                maxTicksLimit: 5,
                callback: (value) => formatValue(value),
              },
            },
            x: {
              type: 'time',
              time: {
                parser: 'hh:mm:ss',
                unit: 'second',
                tooltipFormat: 'MMM DD, H:mm:ss a',
                displayFormats: {
                  second: 'H:mm:ss',
                },
              },
              border: {
                display: false,
              },              
              grid: {
                display: false,
              },
              ticks: {
                autoSkipPadding: 48,
                maxRotation: 0,
              },
            },
          },
          plugins: {
            legend: {
              display: false,
            },
            tooltip: {
              titleFont: {
                weight: '600',
              },
              callbacks: {
                label: (context) => formatValue(context.parsed.y),
              },
            },
          },
          interaction: {
            intersect: false,
            mode: 'nearest',
          },
          animation: false,
          maintainAspectRatio: false,
          resizeDelay: 200,
        },
      })
      // output header values
      handleHeaderValues(props.data, chartValue, chartDeviation)
    })

    onUnmounted(() => chart.destroy())

    watch(
      () => props.data,
      (data) => {
        // update chart
        chart.data = data
        chart.update()
      }
    )

    return {
      canvas,
      chartValue,
      chartDeviation,
    }
  }
}
</script>