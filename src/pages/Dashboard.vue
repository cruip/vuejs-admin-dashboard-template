<template>
  <div class="flex h-screen overflow-hidden">

    <!-- Sidebar -->
    <Sidebar :sidebarOpen="sidebarOpen" @close-sidebar="sidebarOpen = false" />

    <!-- Content area -->
    <div class="relative flex flex-col flex-1 overflow-y-auto overflow-x-hidden">
      
      <!-- Site header -->
      <Header :sidebarOpen="sidebarOpen" @toggle-sidebar="sidebarOpen = !sidebarOpen" />

      <main>
        <div class="px-4 sm:px-6 lg:px-8 py-8 w-full max-w-9xl mx-auto">

          <!-- Welcome banner -->
          <WelcomeBanner />
          
          <!-- Dashboard actions -->
          <div class="sm:flex sm:justify-between sm:items-center mb-8">

            <!-- Left: Avatars -->
            <DashboardAvatars />

            <!-- Right: Actions -->
            <div class="grid grid-flow-col sm:auto-cols-max justify-start sm:justify-end gap-2">

              <!-- Filter button -->
              <FilterButton align="right" />
              <!-- Datepicker built with flatpickr -->
              <Datepicker align="right" />
              <!-- Add view button -->
              <button class="btn bg-indigo-500 hover:bg-indigo-600 text-white">
                  <svg class="w-4 h-4 fill-current opacity-50 shrink-0" viewBox="0 0 16 16">
                      <path d="M15 7H9V1c0-.6-.4-1-1-1S7 .4 7 1v6H1c-.6 0-1 .4-1 1s.4 1 1 1h6v6c0 .6.4 1 1 1s1-.4 1-1V9h6c.6 0 1-.4 1-1s-.4-1-1-1z" />
                  </svg>
                  <span class="hidden xs:block ml-2">Add view</span>
              </button>
            </div>

          </div>

          <!-- Cards -->
          <div class="grid grid-cols-12 gap-6">

            <!-- Line chart (Acme Plus) -->
            <DashboardCard01 />
            <!-- Line chart (Acme Advanced) -->
            <DashboardCard02 />
            <!-- Line chart (Acme Professional) -->
            <DashboardCard03 />
            <!-- Bar chart (Direct vs Indirect) -->
            <DashboardCard04 />
            <!-- Line chart (Real Time Value) -->
            <DashboardCard05 />
            <!-- Doughnut chart (Top Countries) -->
            <DashboardCard06 />
            <!-- Table (Top Channels) -->
            <DashboardCard07 />
            <!-- Line chart (Sales Over Time) -->
            <DashboardCard08 />
            <!-- Stacked bar chart (Sales VS Refunds) -->
            <DashboardCard09 />
            <!-- Card (Customers)  -->
            <DashboardCard10 />
            <!-- Card (Reasons for Refunds)   -->
            <DashboardCard11 /> 
            <!-- Card (Recent Activity) -->
            <DashboardCard12 />
            <!-- Card (Income/Expenses) -->
            <DashboardCard13 />

          </div>

        </div>
      </main>

      <Banner />

    </div> 

  </div>
</template>

<script>
import { ref } from 'vue'
import Sidebar from '../partials/Sidebar.vue'
import Header from '../partials/Header.vue'
import WelcomeBanner from '../partials/dashboard/WelcomeBanner.vue'
import DashboardAvatars from '../partials/dashboard/DashboardAvatars.vue'
import FilterButton from '../components/DropdownFilter.vue'
import Datepicker from '../components/Datepicker.vue'
import DashboardCard01 from '../partials/dashboard/DashboardCard01.vue'
import DashboardCard02 from '../partials/dashboard/DashboardCard02.vue'
import DashboardCard03 from '../partials/dashboard/DashboardCard03.vue'
import DashboardCard04 from '../partials/dashboard/DashboardCard04.vue'
import DashboardCard05 from '../partials/dashboard/DashboardCard05.vue'
import DashboardCard06 from '../partials/dashboard/DashboardCard06.vue'
import DashboardCard07 from '../partials/dashboard/DashboardCard07.vue'
import DashboardCard08 from '../partials/dashboard/DashboardCard08.vue'
import DashboardCard09 from '../partials/dashboard/DashboardCard09.vue'
import DashboardCard10 from '../partials/dashboard/DashboardCard10.vue'
import DashboardCard11 from '../partials/dashboard/DashboardCard11.vue'
import DashboardCard12 from '../partials/dashboard/DashboardCard12.vue'
import DashboardCard13 from '../partials/dashboard/DashboardCard13.vue'
import Banner from '../partials/Banner.vue'

export default {
  name: 'Dashboard',
  components: {
    Sidebar,
    Header,
    WelcomeBanner,
    DashboardAvatars,
    FilterButton,
    Datepicker,
    DashboardCard01,
    DashboardCard02,
    DashboardCard03,
    DashboardCard04,
    DashboardCard05,
    DashboardCard06,
    DashboardCard07,
    DashboardCard08,
    DashboardCard09,
    DashboardCard10,
    DashboardCard11,
    DashboardCard12,
    DashboardCard13,
    Banner,
  },
  setup() {

    const sidebarOpen = ref(false)

    return {
      sidebarOpen,
    }  
  }
}
</script>