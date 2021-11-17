<template>
  <div class="relative inline-flex">
    <button
      ref="trigger"
      class="btn bg-white border-gray-200 hover:border-gray-300 text-gray-500 hover:text-gray-600"
      aria-haspopup="true"
      @click.prevent="dropdownOpen = !dropdownOpen"
      :aria-expanded="dropdownOpen"
    >
      <span class="sr-only">Filter</span><wbr />
      <svg class="w-4 h-4 fill-current" viewBox="0 0 16 16">
        <path d="M9 15H7a1 1 0 010-2h2a1 1 0 010 2zM11 11H5a1 1 0 010-2h6a1 1 0 010 2zM13 7H3a1 1 0 010-2h10a1 1 0 010 2zM15 3H1a1 1 0 010-2h14a1 1 0 010 2z" />
      </svg>
    </button>
    <transition
      enter-active-class="transition ease-out duration-200 transform"
      enter-from-class="opacity-0 -translate-y-2"
      enter-to-class="opacity-100 translate-y-0"
      leave-active-class="transition ease-out duration-200"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div v-show="dropdownOpen" class="origin-top-right z-10 absolute top-full min-w-56 bg-white border border-gray-200 pt-1.5 rounded shadow-lg overflow-hidden mt-1" :class="align === 'right' ? 'right-0' : 'left-0'">
        <div ref="dropdown">
          <div class="text-xs font-semibold text-gray-400 uppercase pt-1.5 pb-2 px-4">Filters</div>
          <ul class="mb-4">
            <li class="py-1 px-3">
              <label class="flex items-center">
                <input type="checkbox" class="form-checkbox" />
                <span class="text-sm font-medium ml-2">Direct VS Indirect</span>
              </label>
            </li>
            <li class="py-1 px-3">
              <label class="flex items-center">
                <input type="checkbox" class="form-checkbox" />
                <span class="text-sm font-medium ml-2">Real Time Value</span>
              </label>
            </li>
            <li class="py-1 px-3">
              <label class="flex items-center">
                <input type="checkbox" class="form-checkbox" />
                <span class="text-sm font-medium ml-2">Top Channels</span>
              </label>
            </li>
            <li class="py-1 px-3">
              <label class="flex items-center">
                <input type="checkbox" class="form-checkbox" />
                <span class="text-sm font-medium ml-2">Sales VS Refunds</span>
              </label>
            </li>
            <li class="py-1 px-3">
              <label class="flex items-center">
                <input type="checkbox" class="form-checkbox" />
                <span class="text-sm font-medium ml-2">Last Order</span>
              </label>
            </li>
            <li class="py-1 px-3">
              <label class="flex items-center">
                <input type="checkbox" class="form-checkbox" />
                <span class="text-sm font-medium ml-2">Total Spent</span>
              </label>
            </li>
          </ul>
          <div class="py-2 px-3 border-t border-gray-200 bg-gray-50">
            <ul class="flex items-center justify-between">
              <li>
                <button class="btn-xs bg-white border-gray-200 hover:border-gray-300 text-gray-500 hover:text-gray-600">Clear</button>
              </li>
              <li>
                <button class="btn-xs bg-indigo-500 hover:bg-indigo-600 text-white" @click="dropdownOpen = false" @focusout="dropdownOpen = false">Apply</button>
              </li>
            </ul>
          </div>          
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue'

export default {
  name: 'DropdownFilter',
  props: ['align'],
  setup() {

    const dropdownOpen = ref(false)
    const trigger = ref(null)
    const dropdown = ref(null)

    // close on click outside
    const clickHandler = ({ target }) => {
      if (!dropdownOpen.value || dropdown.value.contains(target) || trigger.value.contains(target)) return
      dropdownOpen.value = false
    }

    // close if the esc key is pressed
    const keyHandler = ({ keyCode }) => {
      if (!dropdownOpen.value || keyCode !== 27) return
      dropdownOpen.value = false
    }

    onMounted(() => {
      document.addEventListener('click', clickHandler)
      document.addEventListener('keydown', keyHandler)
    })

    onUnmounted(() => {
      document.removeEventListener('click', clickHandler)
      document.removeEventListener('keydown', keyHandler)
    })

    return {
      dropdownOpen,
      trigger,
      dropdown,
    }
  }
}
</script>