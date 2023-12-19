<script setup>
import { ref } from 'vue'
import { Chart } from 'highcharts-vue'

import IconBars from '../components/icons/IconBars.vue'
import SidePanel from '../components/SidePanel.vue'
import SectionDropdown from '../components/SectionDropdown.vue'

const sideSections = ref([
  {
    id: 1,
    name: 'Timeframe',
    visible: true,
    children: [
      {
        name: 'Years',
        options: [2022, 2023],
        selected: []
      },
      {
        name: 'Months',
        options: ['January', 'February'],
        selected: []
      }
    ]
  },
  {
    id: 2,
    name: 'Sources',
    visible: true,
    children: [
      {
        name: 'Electricity',
        options: ['Unmetered Electricity', 'Building Electricity'],
        selected: []
      },
      {
        name: 'Gas',
        options: ['Building Gas'],
        selected: []
      },
      {
        name: 'Other Sources',
        options: ['Building Solar', 'Fleet Road Diesel (DERV)'],
        selected: []
      }
    ]
  }
])

const chartOptionsCost = {
  chart: {
    type: 'pie'
  },
  title: {
    text: 'Cost (Inc VAT) - 2021',
    style: {
      color: '#FF6666'
    }
  },
  credits: {
    enabled: false
  },
  series: [
    {
      name: 'Expenses',
      data: [
        { name: 'Gas', y: 1000, color: '#FF6666' },
        { name: 'Electricity', y: 7000, color: '#FF3333' },
        { name: 'Other', y: 2000, color: '#FF0000' }
      ],
      innerSize: '50%',
      dataLabels: {
        enabled: true,
        format: '<b>{point.name}</b>: {point.y}'
      }
    }
  ]
}

const chartOptionsConsumption = {
  chart: {
    type: 'pie'
  },
  title: {
    text: 'Consumption (kWhs) - 2021',
    style: {
      color: '#66CCFF'
    }
  },
  credits: {
    enabled: false
  },
  series: [
    {
      name: 'Expenses',
      data: [
        { name: 'Gas', y: 3000, color: '#66CCFF' },
        { name: 'Electricity', y: 7000, color: '#3399FF' },
        { name: 'Other', y: 4000, color: '#0066CC' }
      ],
      innerSize: '50%',
      dataLabels: {
        enabled: true,
        format: '<b>{point.name}</b>: {point.y}'
      }
    }
  ]
}

const chartOptionsCarbon = {
  chart: {
    type: 'pie'
  },
  title: {
    text: 'Carbon (Tonnes) - 2021',
    style: {
      color: '#006600'
    }
  },
  credits: {
    enabled: false
  },
  series: [
    {
      name: 'Expenses',
      data: [
        { name: 'Gas', y: 1000, color: '#006600' },
        { name: 'Electricity', y: 5000, color: '#009900' },
        { name: 'Other', y: 2400, color: '#00CC00' }
      ],
      innerSize: '50%',
      dataLabels: {
        enabled: true,
        format: '<b>{point.name}</b>: {point.y}'
      }
    }
  ]
}
</script>

<template>
  <div class="flex flex-row h-screen">
    <side-panel :sections="sideSections" @update:sections="sideSections = $event" />
    <main class="flex flex-col flex-1">
      <header class="flex items-center py-2 text-white bg-black shadow-bottom md:hidden sm:pr-8">
        <div class="relative mr-0.5">
          <div class="inline-flex items-center">
            <button class="p-2" type="button">
              <icon-bars />
            </button>
          </div>
        </div>
      </header>
      <div class="p-2 space-y-3 overflow-auto">
        <div class="p-4 border border-gray-200 rounded-md">
          <h2 class="text-xl font-bold text-center">
            5 years Energy Estimate: <span class="text-blue-400">€6000.20</span>
          </h2>
        </div>

        <section-dropdown>
          <template #title> Totals </template>

          <template #content>
            <div class="grid chart-grid">
              <chart :options="chartOptionsCost"></chart>
              <chart :options="chartOptionsConsumption"></chart>
              <chart :options="chartOptionsCarbon"></chart>
            </div>
          </template>
        </section-dropdown>

        <section-dropdown>
          <template #title> Cost (Inc VAT) - 2021 </template>

          <template #content>
            <div class="flex justify-center py-8 text-gray-300">EMPTY</div>
          </template>
        </section-dropdown>

        <section-dropdown>
          <template #title> Consumption - 2021 </template>

          <template #content>
            <div class="flex justify-center py-8 text-gray-300">EMPTY</div>
          </template>
        </section-dropdown>
      </div>
    </main>
  </div>
</template>
