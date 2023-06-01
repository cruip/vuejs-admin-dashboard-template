<template>
  <div class="grow flex flex-col justify-center">
    <div>
      <canvas ref="canvas" :data="data" :width="width" :height="height"></canvas>
    </div>
    <div class="px-5 pt-2 pb-6">
      <ul ref="legend" class="flex flex-wrap justify-center -m-1"></ul>
    </div>
  </div>  
</template>

<script>
import { ref, watch, onMounted, onUnmounted } from 'vue'
import { useDark } from '@vueuse/core'
import { chartColors } from './ChartjsConfig'

import {
  Chart, DoughnutController, ArcElement, TimeScale, Tooltip,
} from 'chart.js'
import 'chartjs-adapter-moment'

// Import utilities
import { tailwindConfig } from '../utils/Utils'

Chart.register(DoughnutController, ArcElement, TimeScale, Tooltip)

export default {
  name: 'DoughnutChart',
  props: ['data', 'width', 'height'],
  setup(props) {

    const canvas = ref(null)
    const legend = ref(null)
    let chart = null
    const darkMode = useDark()
    const { tooltipTitleColor, tooltipBodyColor, tooltipBgColor, tooltipBorderColor } = chartColors
    
    onMounted(() => {
      const ctx = canvas.value
      chart = new Chart(ctx, {
        type: 'doughnut',
        data: props.data,
        options: {
          cutout: '80%',
          layout: {
            padding: 24,
          },
          plugins: {
            legend: {
              display: false,
            },
            tooltip: {
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
          animation: {
            duration: 500,
          },
          maintainAspectRatio: false,
          resizeDelay: 200,
        },
        plugins: [{
          id: 'htmlLegend',
          afterUpdate(c, args, options) {
            const ul = legend.value
            if (!ul) return
            // Remove old legend items
            while (ul.firstChild) {
              ul.firstChild.remove()
            }
            // Reuse the built-in legendItems generator
            const items = c.options.plugins.legend.labels.generateLabels(c)
            items.forEach((item) => {
              const li = document.createElement('li')
              li.style.margin = tailwindConfig().theme.margin[1]
              // Button element
              const button = document.createElement('button')
              button.classList.add('btn-xs', 'bg-white', 'dark:bg-slate-800', 'text-slate-500', 'dark:text-slate-400', 'border', 'border-slate-200', 'dark:border-slate-700', 'shadow-md')
              button.style.opacity = item.hidden ? '.3' : ''
              button.onclick = () => {
                c.toggleDataVisibility(item.index, !item.index)
                c.update()
              }
              // Color box
              const box = document.createElement('span')
              box.style.display = 'block'
              box.style.width = tailwindConfig().theme.width[2]
              box.style.height = tailwindConfig().theme.height[2]
              box.style.backgroundColor = item.fillStyle
              box.style.borderRadius = tailwindConfig().theme.borderRadius.sm
              box.style.marginRight = tailwindConfig().theme.margin[1]
              box.style.pointerEvents = 'none'
              // Label
              const label = document.createElement('span')
              label.style.display = 'flex'
              label.style.alignItems = 'center'
              const labelText = document.createTextNode(item.text)
              label.appendChild(labelText)
              li.appendChild(button)
              button.appendChild(box)
              button.appendChild(label)
              ul.appendChild(li)
            })
          },
        }],        
      })
    })

    onUnmounted(() => chart.destroy())

    watch(
      () => darkMode.value,
      () => {
        if (darkMode.value) {
          chart.options.plugins.tooltip.titleColor = tooltipTitleColor.dark
          chart.options.plugins.tooltip.bodyColor = tooltipBodyColor.dark
          chart.options.plugins.tooltip.backgroundColor = tooltipBgColor.dark
          chart.options.plugins.tooltip.borderColor = tooltipBorderColor.dark
        } else {
          chart.options.plugins.tooltip.titleColor = tooltipTitleColor.light
          chart.options.plugins.tooltip.bodyColor = tooltipBodyColor.light
          chart.options.plugins.tooltip.backgroundColor = tooltipBgColor.light
          chart.options.plugins.tooltip.borderColor = tooltipBorderColor.light
        }
        chart.update('none')
      })         

    return {
      canvas,
      legend,
    }
  }
}
</script>