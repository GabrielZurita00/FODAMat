<template>
    <div>
      <radar-chart
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
    </div>
  </template>
  
  <script>
 
import { Radar } from 'vue-chartjs'
import '@fontsource/poppins';
import * as ChartImport from 'chart.js';
import ChartDataLabels from 'chartjs-plugin-datalabels';
const {
  Chart,
  RadialLinearScale,
  PointElement,
  LineElement,
  Filler,
  Tooltip,
  Legend
} = ChartImport.default ? ChartImport.default : ChartImport;
Chart.register(RadialLinearScale, PointElement, LineElement, Filler, Tooltip, Legend, ChartDataLabels);
Chart.defaults.font.size = 16;
Chart.defaults.font.family = "Poppins"  
export default {
    props: {
      areas: {
          type: Array,
          required: true
      }
  },
    components: {
      RadarChart: Radar
    },
    data() {
      return {
        chartOptions: {
          responsive: true,
          scales: {
            r: {
              beginAtZero: true
            }
          },
          plugins: {
            datalabels: {
        display: false,
    },
          }
        },
      chartId: 'radar-chart',
      datasetIdKey: 'label',
      plugins: [],
      cssClasses: '',
      styles: {},
      width: 400,
      height: 400,
      chartData: {
        labels: [
          //Areas
        ],
        datasets: [
          {
            label: 'Fortalezas',
            data: [],
            backgroundColor: 'rgba(198,229,177,0.2)',
            borderColor: 'rgb(198,229,177)',
            pointBorderColor: '#fff',
            pointHoverBackgroundColor: '#fff',
            pointHoverBorderColor: 'rgba(198,229,177,1.000)'
          },
          {
            label: 'Debilidades',
            data: [],
            backgroundColor: 'rgba(249,249,177,0.2)',
            borderColor: 'rgb(249,249,177)',
            pointBorderColor: '#fff',
            pointHoverBackgroundColor: '#fff',
            pointHoverBorderColor: 'rgba(249,249,177,1.000)'
          },
          {
            label: 'Oportunidades',
            data: [],
            backgroundColor: 'rgba(157,191,229,0.2)',
            borderColor: 'rgb(157,191,229)',
            pointBorderColor: '#fff',
            pointHoverBackgroundColor: '#fff',
            pointHoverBorderColor: 'rgba(157,191,229,1.000)'
          },
          {
            label: 'Amenazas',
            data: [],
            backgroundColor: 'rgba(228,111,108,0.2)',
            borderColor: 'rgb(228,111,108)',
            pointBorderColor: '#fff',
            pointHoverBackgroundColor: '#fff',
            pointHoverBorderColor: 'rgba(228,111,108,1.000)'
          },
        ],
      }
      };
    },
    watch:{
      areas(newValue){
        let fValues = []
        let dValues = []
        let oValues = []
        let aValues = []
        this.chartData.labels = []
        this.chartData.datasets[0].data=[]
        this.chartData.datasets[1].data=[]
        this.chartData.datasets[2].data=[]
        this.chartData.datasets[3].data=[]
        for (let i=0;i<=newValue.length;i++){
          this.chartData.labels.push(`Área ${i+1}`)
          fValues.push(this.FPercent(newValue[i]))
          dValues.push(this.DPercent(newValue[i]))
          oValues.push(this.OPercent(newValue[i]))
          aValues.push(this.APercent(newValue[i]))
        }
        this.chartData.datasets[0].data= fValues
        this.chartData.datasets[1].data= dValues
        this.chartData.datasets[2].data= oValues
        this.chartData.datasets[3].data= aValues
      }
    },
    methods: {
      FPercent(area){
        return (area.totalf+area.totald+area.totalo+area.totala)!=0 ? Math.round((((area.totalf*100)/(area.totalf+area.totald+area.totalo+area.totala))+Number.EPSILON)) : 0
      },
      DPercent(area){
        return (area.totalf+area.totald+area.totalo+area.totala)!=0 ? Math.round((((area.totald*100)/(area.totalf+area.totald+area.totalo+area.totala))+Number.EPSILON)) : 0
      },
      OPercent(area){
        return (area.totalf+area.totald+area.totalo+area.totala)!=0 ? Math.round((((area.totalo*100)/(area.totalf+area.totald+area.totalo+area.totala))+Number.EPSILON)) : 0
      },
      APercent(area){
        return (area.totalf+area.totald+area.totalo+area.totala)!=0 ? Math.round((((area.totala*100)/(area.totalf+area.totald+area.totalo+area.totala))+Number.EPSILON)) : 0
      }
    },
    mounted(){
      let fValues = []
      let dValues = []
      let oValues = []
      let aValues = []
      this.chartData.labels = []
      for (let i=0;i<this.areas.length;i++){
        this.chartData.labels.push(`Área ${i+1}`)
        fValues.push(this.FPercent(this.areas[i]))
        dValues.push(this.DPercent(this.areas[i]))
        oValues.push(this.OPercent(this.areas[i]))
        aValues.push(this.APercent(this.areas[i]))
      }
      this.chartData.datasets[0].data= fValues
      this.chartData.datasets[1].data= dValues
      this.chartData.datasets[2].data= oValues
      this.chartData.datasets[3].data= aValues
    }
  };
  </script>
  