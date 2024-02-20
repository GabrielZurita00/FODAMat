<template>
    <pie-chart
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
  import { Pie } from 'vue-chartjs'
  import '@fontsource/poppins';
  import ChartDataLabels from 'chartjs-plugin-datalabels';
  import * as ChartImport from 'chart.js';
  
  
  const {
  Chart,
  Title,
  Tooltip,
  Legend,
  ArcElement,
  CategoryScale,
} = ChartImport.default ? ChartImport.default : ChartImport;

  Chart.register(Title, Tooltip, Legend, ArcElement, CategoryScale, ChartDataLabels)
  Chart.defaults.font.size = 16;
Chart.defaults.font.family = "Poppins"
  export default {
    props:{
      totalf: {
        type: Array,
        required: true
      },
      totalo: {
        type: Array,
        required: true
      },
      totald: {
        type: Array,
        required: true
      },
      totala: {
        type: Array,
        required: true
      },
      graphW: {
        type: Number,
        required: true
      },
      graphH: {
        type: Number,
        required: true
      },
    },
    components: {
      PieChart: Pie
    },
    data() {
      return {
        total: 0,
        chartData: {
          labels: ['Fortalezas', 'Debilidades', 'Oportunidades', 'Amenazas'],
          datasets: [
            {
              backgroundColor: ['rgba(198,229,177,1.000)', 'rgba(249,249,177,1.000)', 'rgba(157,191,229,1.000)', 'rgba(228,111,108,1.000)'],
              data: []
            }
          ],
          fTotal: 0,
          oTotal: 0,
          dTotal: 0,
          aTotal: 0
        },
        chartOptions: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            tooltip: {
              enabled: true,
              callbacks: {
                label: function(context) {
                  let label = context.label || '';
                  let sum=0
                  for (const element of context.dataset.data){
                    sum += element
                  }
                  if (label){
                    label+=': '; 
                    label+=context.parsed
                    label+=' ('
                    label+=sum!=0 ? Math.round((((context.parsed*100)/sum)+Number.EPSILON)*100)/100 : 0
                    label+='%)'
                  }
                  console.log(label);
                  return label
                }
              },
              
            },
            datalabels: {
        color: '#000',
        formatter: function (value, context) {
          let sum=0
          for (const element of context.dataset.data){
            sum += element
          }
          return (sum!=0 ? Math.round((((value*100)/sum)+Number.EPSILON)*100)/100 : 0) + '%' 
        },
        font: {
          weight: 'bold',
          size: 22,
        }
      }
          }
        },
        chartId:  'pie-chart',
        datasetIdKey: 'label',
        width: this.graphW,
        height: this.graphH,
        cssClasses: '',
        styles: {},
        plugins: []
      }
    },
    watch: {
      totalf(newValue){
        this.fTotal = newValue.reduce((a, b) => a + b, 0);
        this.chartData.datasets[0].data=[]
        this.total = this.fTotal+this.oTotal+this.dTotal+this.aTotal
        this.chartData.datasets[0].data.push(this.fTotal)
        this.chartData.datasets[0].data.push(this.dTotal)
        this.chartData.datasets[0].data.push(this.oTotal)
        this.chartData.datasets[0].data.push(this.aTotal)
      },
      totald(newValue){
        this.dTotal = newValue.reduce((a, b) => a + b, 0);
        this.chartData.datasets[0].data=[]
        this.total = this.fTotal+this.oTotal+this.dTotal+this.aTotal
        this.chartData.datasets[0].data.push(this.fTotal)
        this.chartData.datasets[0].data.push(this.dTotal)
        this.chartData.datasets[0].data.push(this.oTotal)
        this.chartData.datasets[0].data.push(this.aTotal)
      },
      totalo(newValue){
        this.oTotal = newValue.reduce((a, b) => a + b, 0);
        this.chartData.datasets[0].data=[]
        this.total = this.fTotal+this.oTotal+this.dTotal+this.aTotal
        this.chartData.datasets[0].data.push(this.fTotal)
        this.chartData.datasets[0].data.push(this.dTotal)
        this.chartData.datasets[0].data.push(this.oTotal)
        this.chartData.datasets[0].data.push(this.aTotal)
      },
      totala(newValue){
        this.aTotal = newValue.reduce((a, b) => a + b, 0);
        this.chartData.datasets[0].data=[]
        this.total = this.fTotal+this.oTotal+this.dTotal+this.aTotal
        this.chartData.datasets[0].data.push(this.fTotal)
        this.chartData.datasets[0].data.push(this.dTotal)
        this.chartData.datasets[0].data.push(this.oTotal)
        this.chartData.datasets[0].data.push(this.aTotal)
      }
    },
    mounted(){
      this.fTotal = this.totalf.reduce((a, b) => a + b, 0);
      this.oTotal = this.totalo.reduce((a, b) => a + b, 0);
      this.dTotal = this.totald.reduce((a, b) => a + b, 0);
      this.aTotal = this.totala.reduce((a, b) => a + b, 0);
      this.chartData.datasets[0].data=[]
      this.total = this.fTotal+this.oTotal+this.dTotal+this.aTotal
      this.chartData.datasets[0].data.push(this.fTotal)
      this.chartData.datasets[0].data.push(this.dTotal)
      this.chartData.datasets[0].data.push(this.oTotal)
      this.chartData.datasets[0].data.push(this.aTotal)
    }
  }
  </script>