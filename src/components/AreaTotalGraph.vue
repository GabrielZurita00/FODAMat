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
          labels: [],
          datasets: [
            {
              label: 'Total',
              data: [], backgroundColor: [
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
      ]
            },
          ],
        },
        values:[]
      };
    },
    watch:{
        totalf(newValue) {
          this.values=newValue.concat(this.totald,this.totalo,this.totala)
          this.chartData.labels=[]
          for (let i=0;i<newValue.length;i++){
            this.chartData.labels.push(`F${i+1}`)
          }
          for (let i=0;i<this.totald.length;i++){
            this.chartData.labels.push(`D${i+1}`)
          }
          for (let i=0;i<this.totalo.length;i++){
            this.chartData.labels.push(`O${i+1}`)
          }
          for (let i=0;i<this.totala.length;i++){
            this.chartData.labels.push(`A${i+1}`)
          }
          this.chartData.datasets[0].data = this.values
        },
        totalo(newValue) {
          this.values=this.totalf.concat(this.totald,newValue,this.totala)
          this.chartData.labels=[]
          for (let i=0;i<this.totalf.length;i++){
            this.chartData.labels.push(`F${i+1}`)
          }
          for (let i=0;i<this.totald.length;i++){
            this.chartData.labels.push(`D${i+1}`)
          }
          for (let i=0;i<newValue.length;i++){
            this.chartData.labels.push(`O${i+1}`)
          }
          for (let i=0;i<this.totala.length;i++){
            this.chartData.labels.push(`A${i+1}`)
          }
          this.chartData.datasets[0].data = this.values
        },
        totald(newValue) {
          this.values=this.totalf.concat(newValue,this.totalo,this.totala)
          this.chartData.labels=[]
          for (let i=0;i<this.totalf.length;i++){
            this.chartData.labels.push(`F${i+1}`)
          }
          for (let i=0;i<newValue.length;i++){
            this.chartData.labels.push(`D${i+1}`)
          }
          for (let i=0;i<this.totalo.length;i++){
            this.chartData.labels.push(`O${i+1}`)
          }
          for (let i=0;i<this.totala.length;i++){
            this.chartData.labels.push(`A${i+1}`)
          }
          this.chartData.datasets[0].data = this.values
        },
        totala(newValue) {
          this.values=this.totalf.concat(this.totald,this.totalo,newValue)
          this.chartData.labels=[]
          for (let i=0;i<this.totalf.length;i++){
            this.chartData.labels.push(`F${i+1}`)
          }
          for (let i=0;i<this.totald.length;i++){
            this.chartData.labels.push(`D${i+1}`)
          }
          for (let i=0;i<this.totalo.length;i++){
            this.chartData.labels.push(`O${i+1}`)
          }
          for (let i=0;i<newValue.length;i++){
            this.chartData.labels.push(`A${i+1}`)
          }
          this.chartData.datasets[0].data = this.values
        },
    },
    mounted(){
        //this.chartData.datasets[0].data=[this.totals]
        this.values=this.totalf.concat(this.totald,this.totalo,this.totala)
        this.chartData.labels=[]
          for (let i=0;i<this.totalf.length;i++){
            this.chartData.labels.push(`F${i+1}`)
          }
          for (let i=0;i<this.totald.length;i++){
            this.chartData.labels.push(`D${i+1}`)
          }
          for (let i=0;i<this.totalo.length;i++){
            this.chartData.labels.push(`O${i+1}`)
          }
          for (let i=0;i<this.totala.length;i++){
            this.chartData.labels.push(`A${i+1}`)
          }
          this.chartData.datasets[0].data = this.values
    }
  };
  </script>