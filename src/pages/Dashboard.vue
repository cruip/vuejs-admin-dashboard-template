<template>
  <div class="flex h-screen overflow-hidden">

    <!-- Sidebar -->
    <Sidebar :sidebarOpen="sidebarOpen" @close-sidebar="sidebarOpen = false" />

    <!-- Content area -->
    <div class="relative flex flex-col flex-1 overflow-y-auto overflow-x-hidden">
      
      <!-- Site header -->
      <Header :sidebarOpen="sidebarOpen" @toggle-sidebar="sidebarOpen = !sidebarOpen" />
      <loading v-model:active="isLoading"
        :is-full-page="true"/>
      <main>
        <div class="px-4 sm:px-6 lg:px-8 py-8 w-full max-w-9xl mx-auto">

          <!-- Dashboard actions -->
          <div class="sm:items-center mb-8">
            <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md">
              <span class="flex select-none items-center pl-3 text-gray-500 sm:text-sm">Select Clinic/</span>
              <select type="text" v-model="selectedClinic" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6">
                <option v-for="clinic in clinics" :key="clinic.id" :value="clinic">{{clinic.name}}</option>
              </select>
            </div>

            <!-- Right: Actions -->
            <div class="grid grid-flow-col sm:auto-cols-max justify-start sm:justify-start gap-2 mt-3">
              <div class="flex flex-col col-span-full sm:col-span-6 xl:col-span-4 bg-white dark:bg-slate-800 shadow-lg rounded-sm border border-slate-200 dark:border-slate-700">
                <!-- Chart built with Chart.js 3 -->
                <!-- Change the height attribute to adjust the chart height -->
                <DoughnutChart :data="chartData" width="389" height="260" v-show="selectedClinic !== {}"/>
              </div>
            </div>

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
import { tailwindConfig } from '../utils/Utils'
import Banner from '../partials/Banner.vue'
import axios from 'axios';
import DoughnutChart from '../charts/DoughnutChart.vue'
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/css/index.css';

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
    DoughnutChart,
    Loading,
  },
  setup() {

    const sidebarOpen = ref(false)

    return {
      sidebarOpen,
    }  
  },
  data() {
    return {
      clinics: [],
      selectedClinic: {},
      chartData: ref({}),
      isLoading: false
    }
  },
  mounted() {
    this.isLoading = true;
    axios.get(`https://be8you77we.execute-api.us-east-1.amazonaws.com/clinics`).then((res) => {
      this.clinics = res.data.data[0][1];
      this.isLoading = false;
    }).catch((error) => this.isLoading = false)
  },
  watch: {
    clinics: function(val, oldVal) {

    },
    selectedClinic: function(val, oldVal) {
      this.isLoading = true;
      axios.get(`https://be8you77we.execute-api.us-east-1.amazonaws.com/appointment/${val.id}`).then((res) => {
        this.chartData = ref({
          labels: ['Appointment Completed', 'Appointment Cancelled', 'Total'],
          datasets: [
            {
              label: 'Appointment Details',
              data: [
                res.data.data[0][1]['appointmentsCompleted'], res.data.data[0][1]['appointmentsCancelled'], res.data.data[0][1]['total']
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
              borderWidth: 0,
            },
          ],
        });

        this.isLoading = false;
      }).catch((error) => this.isLoading = false)
    }
  }
}
</script>