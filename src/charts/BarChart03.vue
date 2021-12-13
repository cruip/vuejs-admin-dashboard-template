<template>
  <div class="grow flex flex-col justify-center">
    <div>
      <canvas ref="canvas" :data="data" :width="width" :height="height"></canvas>
    </div>
    <div class="px-5 pt-2 pb-2">
      <ul ref="legend" class="text-sm divide-y divide-gray-100"></ul>
      <ul class="text-sm divide-y divide-gray-100"></ul>
    </div>
  </div>  
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue'
import {
  Chart, BarController, BarElement, LinearScale, CategoryScale, Tooltip, Legend,
} from 'chart.js'
import 'chartjs-adapter-moment'

// Import utilities
import { tailwindConfig } from '../utils/Utils'

Chart.register(BarController, BarElement, LinearScale, CategoryScale, Tooltip, Legend)

export default {
  name: 'BarChart03',
  props: ['data', 'width', 'height'],
  setup(props) {

    const canvas = ref(null)
    const legend = ref(null)
    let chart = null
    
    onMounted(() => {

      // Calculate sum of values
      const reducer = (accumulator, currentValue) => accumulator + currentValue
      const values = props.data.datasets.map(x => x.data.reduce(reducer))
      const max = values.reduce(reducer)    

      const ctx = canvas.value
      chart = new Chart(ctx, {
        type: 'bar',
        data: props.data,
        options: {
          indexAxis: 'y',
          layout: {
            padding: {
              top: 12,
              bottom: 12,
              left: 20,
              right: 20,
            },
          },
          scales: {
            x: {
              stacked: true,
              display: false,
              max: max,
            },
            y: {
              stacked: true,
              display: false,
            },
          },
          plugins: {
            legend: {
              display: false,
            },
            tooltip: {
              callbacks: {
                title: () => false, // Disable tooltip title
                label: (context) => context.parsed.x,
              },
            },
          },
          interaction: {
            intersect: false,
            mode: 'nearest'
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
              li.style.display = 'flex'
              li.style.justifyContent = 'space-between'
              li.style.alignItems = 'center'
              li.style.paddingTop = tailwindConfig().theme.padding[2.5]
              li.style.paddingBottom = tailwindConfig().theme.padding[2.5]
              const wrapper = document.createElement('div')
              wrapper.style.display = 'flex'
              wrapper.style.alignItems = 'center'
              const box = document.createElement('div')
              box.style.width = tailwindConfig().theme.width[3]
              box.style.height = tailwindConfig().theme.width[3]
              box.style.borderRadius = tailwindConfig().theme.borderRadius.sm
              box.style.marginRight = tailwindConfig().theme.margin[3]
              box.style.backgroundColor = item.fillStyle
              const label = document.createElement('div')
              const value = document.createElement('div')
              value.style.fontWeight = tailwindConfig().theme.fontWeight.medium
              value.style.marginLeft = tailwindConfig().theme.margin[3]
              value.style.color = item.text === 'Other' ? tailwindConfig().theme.colors.gray[400] : item.fillStyle
              const theValue = c.data.datasets[item.datasetIndex].data.reduce((a, b) => a + b, 0)
              const valueText = document.createTextNode(`${parseInt(theValue / max * 100)}%`)
              const labelText = document.createTextNode(item.text)
              value.appendChild(valueText)
              label.appendChild(labelText)
              ul.appendChild(li)
              li.appendChild(wrapper)
              li.appendChild(value)
              wrapper.appendChild(box)
              wrapper.appendChild(label)
            })
          },
        }],     
      })
    })

    onUnmounted(() => chart.destroy())

    return {
      canvas,
      legend,
    }
  }
}
</script>