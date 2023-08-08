<template>
    <div>
      <radar-chart
        :chart-data="chartData"
        :chart-options="chartOptions"
        :width="400"
        :height="400"
      />
    </div>
  </template>
  
  <script>
//https://codesandbox.io/s/apertureless-vue-chartjs-radar-8rlwjt?file=/src/App.vue
 import {
  Chart,
  RadialLinearScale,
  PointElement,
  LineElement,
  Filler,
  Tooltip,
  Legend
} from 'chart.js'
import { Radar } from 'vue-chartjs'
import '@fontsource/poppins';
import * as ChartImport from 'chart.js';
const {
  Chart,
  RadialLinearScale,
  PointElement,
  LineElement,
  Filler,
  Tooltip,
  Legend
} = ChartImport;
Chart.register(Chart, RadialLinearScale, PointElement, LineElement, Filler, Tooltip, Legend);
Chart.defaults.font.size = 16;
Chart.defaults.font.family = "Poppins"  
export default {
    props: {
      fpercent: {
          type: Array,
          required: true
      },
      dpercent: {
          type: Array,
          required: true
      },
      opercent: {
          type: Array,
          required: true
      },
      apercent: {
          type: Array,
          required: true
      }
  },
    components: {
      RadarChart: Radar
    },
    data() {
      return {
        chartData: {
          labels: ['Total F', 'Total D', 'Total O', 'Total A'],
          datasets: [
            {
              data: [
                this.area.totalf,
                this.area.totald,
                this.area.totalo,
                this.area.totala
              ],
              backgroundColor: 'rgba(75, 192, 192, 0.5)',
              borderColor: 'rgba(75, 192, 192, 1)',
              borderWidth: 2,
              label: 'Data'
            }
          ]
        },
        chartOptions: {
          responsive: true,
          scales: {
            r: {
              beginAtZero: true
            }
          }
        }
      };
    }
  };
  </script>
  