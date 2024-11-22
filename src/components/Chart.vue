<template>
  <div>
    <Line :data="chartData2" :options="options" />
  </div>
</template>

<script lang="ts">
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js'
import { Line } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)

export default {
  name: 'Chart',
  components: { Line },
  props: {
    daily: Array,
  },
  computed: {
    chartData2() {
      return {
        labels: this.daily.map(o => o.date),   //  labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
        datasets: [
          {
            label: 'Max Temperature',
            backgroundColor: '#280707',
            data: this.daily.map(o => o.temp_max)  //  data: [40, 39, 10, 40, 39, 80, 40]
          }
        ]
      }
    },
  },
  data() {
    return {
      options: {
        responsive: true,
        maintainAspectRatio: false
      }
    }
  },
}
</script>
