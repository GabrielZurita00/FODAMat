<template>
    <Pie
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
    name: 'PieChart',
    props:{
      fTotal: {
        type: Number,
        required: true
      },
      oTotal: {
        type: Number,
        required: true
      },
      dTotal: {
        type: Number,
        required: true
      },
      aTotal: {
        type: Number,
        required: true
      }
    },
    components: {
      Pie
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
          ]
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
                  for (let i=0; i<context.dataset.data.length;i++){
                    sum += context.dataset.data[i]
                  }
                  if (label){
                    label+=': '; 
                    label+=context.parsed
                    label+=' ('
                    label+=sum!=0 ? Math.round((((context.parsed*100)/sum)+Number.EPSILON)*100)/100 : 0
                    label+='%)'
                  }
                  return label
                }
              },
              
            },
            datalabels: {
        color: '#000',
        formatter: function (value, context) {
          let sum=0
          for (let i=0; i<context.dataset.data.length;i++){
            sum += context.dataset.data[i]
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
        width: 400,
        height: 300,
        cssClasses: '',
        styles: {},
        plugins: []
      }
    },
    watch: {
      fTotal(newValue){
        this.chartData.datasets[0].data=[]
        this.total = newValue+this.oTotal+this.dTotal+this.aTotal
        this.chartData.datasets[0].data.push(newValue)
        this.chartData.datasets[0].data.push(this.dTotal)
        this.chartData.datasets[0].data.push(this.oTotal)
        this.chartData.datasets[0].data.push(this.aTotal)
      },
      dTotal(newValue){
        this.chartData.datasets[0].data=[]
        this.total = this.fTotal+this.oTotal+newValue+this.aTotal
        this.chartData.datasets[0].data.push(this.fTotal)
        this.chartData.datasets[0].data.push(newValue)
        this.chartData.datasets[0].data.push(this.oTotal)
        this.chartData.datasets[0].data.push(this.aTotal)
      },
      oTotal(newValue){
        this.chartData.datasets[0].data=[]
        this.total = this.fTotal+newValue+this.dTotal+this.aTotal
        this.chartData.datasets[0].data.push(this.fTotal)
        this.chartData.datasets[0].data.push(this.dTotal)
        this.chartData.datasets[0].data.push(this.newValue)
        this.chartData.datasets[0].data.push(this.aTotal)
      },
      aTotal(newValue){
        this.chartData.datasets[0].data=[]
        this.total = this.fTotal+this.oTotal+this.dTotal+newValue
        this.chartData.datasets[0].data.push(this.fTotal)
        this.chartData.datasets[0].data.push(this.dTotal)
        this.chartData.datasets[0].data.push(this.oTotal)
        this.chartData.datasets[0].data.push(newValue)
      }
    },
    methods : {
      percent(value){
        return this.total!=0 ? Math.round((((value*100)/this.total)+Number.EPSILON)*100)/100 : 0
      }
    },
    mounted(){
      this.chartData.datasets[0].data=[]
      this.total = this.fTotal+this.oTotal+this.dTotal+this.aTotal
      this.chartData.datasets[0].data.push(this.fTotal)
      this.chartData.datasets[0].data.push(this.dTotal)
      this.chartData.datasets[0].data.push(this.oTotal)
      this.chartData.datasets[0].data.push(this.aTotal)
    }
  }
  </script>