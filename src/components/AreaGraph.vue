<template>
  <bar-chart
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="width"
    :height="height"
  />
  
</template>

<script>
import { Bar } from 'vue-chartjs';
import * as ChartImport from 'chart.js';
const {
  Chart,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} = ChartImport;

// Register the required plugins globally for all chart instances.
Chart.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

export default {
  props: {
      totalf: {
          type: Number,
          required: true
      },
      totalo: {
          type: Number,
          required: true
      },
      totald: {
          type: Number,
          required: true
      },
      totala: {
          type: Number,
          required: true
      }
  },
  components: {
    BarChart: Bar
  },
  data() {
    return {
      
      chartOptions: {
        responsive: false,
        scales: {
          yAxes: [
            {
              scaleLabel: {
                display: true,
                labelString: 'Success frequency',
              },
            },
          ],
          xAxes: [
            {
              scaleLabel: {
                display: true,
                labelString: 'Attempt number',
              },
            },
          ],
        },
      },
      chartId: 'bar-chart',
      datasetIdKey: 'label',
      plugins: [],
      cssClasses: '',
      styles: {},
      width: 400,
      height: 400,
      chartData: {
        labels: ["Fortaleza", "Debilidad", "Amenaza", "Oportunidad"],
        datasets: [
          {
            label: 'test data',
              data: [],
              //Colores del grafico
              backgroundColor: [
      'rgba(198,229,177,1.000)',
      'rgba(249,249,177,1.000)',
      'rgba(228,111,108,1.000)',
      'rgba(157,191,229,1.000)',
      
    ],
    borderColor: [
      'rgb(157,191,229)',
      'rgb(249,249,177)',  
      'rgb(198,229,177)',
      'rgb(228,111,108)',
      ] },
        ],
      },
    };
  },
  watch:{
      totalf(newValue) {
          this.chartData.datasets[0].data=[newValue, this.totald, this.totala, this.totalo]
          
      },
      totalo(newValue) {
          this.chartData.datasets[0].data=[this.totalf, this.totald, this.totala, newValue]
      },
      totald(newValue) {
          this.chartData.datasets[0].data=[this.totalf, newValue, this.totala, this.totalo]
      },
      totala(newValue) {
          this.chartData.datasets[0].data=[this.totalf, this.totald, newValue, this.totalo]
      }
  },
  mounted(){
      this.chartData.datasets[0].data=[this.totalf, this.totald, this.totala, this.totalo]
  }
};
</script>