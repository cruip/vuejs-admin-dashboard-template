<template>
  <div class="px-5 py-3">
    <div class="flex items-start">
      <div class="text-3xl font-bold text-slate-800 dark:text-slate-100 mr-2 tabular-nums">$<span ref="chartValue">57.81</span></div>
      <div ref="chartDeviation" class="text-sm font-semibold text-white px-1.5 rounded-full"></div>
    </div>
  </div>
  <div class="grow">
    <canvas ref="canvas" :width="width" :height="height"></canvas>
  </div>
</template>

<script>
import { ref, watch, onMounted, onUnmounted } from 'vue'
import { useDark } from '@vueuse/core'
import { chartColors } from './ChartjsConfig'

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
    const darkMode = useDark()
    const { textColor, gridColor, tooltipTitleColor, tooltipBodyColor, tooltipBgColor, tooltipBorderColor } = chartColors

    // function that updates header values
    const handleHeaderValues = (data, chartValue, chartDeviation) => {
      const currentValue = data.datasets[0].data[data.datasets[0].data.length - 1]
      const previousValue = data.datasets[0].data[data.datasets[0].data.length - 2]
      const diff = ((currentValue - previousValue) / previousValue) * 100
      chartValue.value.innerHTML = data.datasets[0].data[data.datasets[0].data.length - 1]
      if (diff < 0) {
        chartDeviation.value.style.backgroundColor = tailwindConfig().theme.colors.amber[500]
      } else {
        chartDeviation.value.style.backgroundColor = tailwindConfig().theme.colors.emerald[500]
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
                color: darkMode.value ? textColor.dark : textColor.light,
              },
              grid: {
                color: darkMode.value ? gridColor.dark : gridColor.light,
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
                color: darkMode.value ? textColor.dark : textColor.light,
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
              titleColor: darkMode.value ? tooltipTitleColor.dark : tooltipTitleColor.light,
              bodyColor: darkMode.value ? tooltipBodyColor.dark : tooltipBodyColor.light,
              backgroundColor: darkMode.value ? tooltipBgColor.dark : tooltipBgColor.light,
              borderColor: darkMode.value ? tooltipBorderColor.dark : tooltipBorderColor.light,               
            },
          },
          interaction: {
            intersect: false,
            mode: 'nearest',
          },
          animation: false,
          maintainAspectRatio: false,
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
        // update header values
        handleHeaderValues(data, chartValue, chartDeviation)        
      }
    )

    watch(
      () => darkMode.value,
      () => {
        if (darkMode.value) {
          chart.options.scales.x.ticks.color = textColor.dark
          chart.options.scales.y.ticks.color = textColor.dark
          chart.options.scales.y.grid.color = gridColor.dark
          chart.options.plugins.tooltip.titleColor = tooltipTitleColor.dark
          chart.options.plugins.tooltip.bodyColor = tooltipBodyColor.dark
          chart.options.plugins.tooltip.backgroundColor = tooltipBgColor.dark
          chart.options.plugins.tooltip.borderColor = tooltipBorderColor.dark
        } else {
          chart.options.scales.x.ticks.color = textColor.light
          chart.options.scales.y.ticks.color = textColor.light
          chart.options.scales.y.grid.color = gridColor.light
          chart.options.plugins.tooltip.titleColor = tooltipTitleColor.light
          chart.options.plugins.tooltip.bodyColor = tooltipBodyColor.light
          chart.options.plugins.tooltip.backgroundColor = tooltipBgColor.light
          chart.options.plugins.tooltip.borderColor = tooltipBorderColor.light
        }
        chart.update('none')
      })    

    return {
      canvas,
      chartValue,
      chartDeviation,
    }
  }
}
</script>