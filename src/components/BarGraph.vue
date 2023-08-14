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
import ChartDataLabels from 'chartjs-plugin-datalabels';
import '@fontsource/poppins';
const {
  Chart,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} = ChartImport.default ? ChartImport.default : ChartImport;

// Register the required plugins globally for all chart instances.
Chart.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, ChartDataLabels);
Chart.defaults.font.size = 16;
Chart.defaults.font.family = "Poppins"

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
        responsive: true,
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
        plugins:{
          legend: {
            display: false
          },
          datalabels: {
        color: '#000',
        font: {
          weight: 'bold',
          size: 22,
          family: "Poppins"
        }
      }
        },
        tooltips:{
          callbacks:{
            label: function(tooltipItem) {
              return tooltipItem.yLabel;
            }
          }
        }
      },
      chartId: 'bar-chart',
      datasetIdKey: 'label',
      plugins: [
        {legend: {
          display: false
        }
      }
      ],
      cssClasses: '',
      styles: {},
      width: 400,
      height: 250,
      chartData: {
        labels: [],
        datasets: [
          {
            data: [],
            backgroundColor: [],
            borderColor: []
          },
        ]
      },
      values:[]
    };
  },
  watch:{
      totalf(newValue) {
        this.values=newValue.concat(this.totald,this.totalo,this.totala)
        this.chartData.labels=[]
        this.chartData.datasets[0].backgroundColor=[]
        this.chartData.datasets[0].borderColor=[]
        for (let i=0;i<newValue.length;i++){
          this.chartData.labels.push(`F${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(198,229,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(198,229,177)')
        }
        for (let i=0;i<this.totald.length;i++){
          this.chartData.labels.push(`D${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(249,249,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(249,249,177)')
        }
        for (let i=0;i<this.totalo.length;i++){
          this.chartData.labels.push(`O${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(157,191,229,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(157,191,229)')
        }
        for (let i=0;i<this.totala.length;i++){
          this.chartData.labels.push(`A${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(228,111,108,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(228,111,108)')
        }
        this.chartData.datasets[0].data = this.values
      },
      totalo(newValue) {
        this.values=this.totalf.concat(this.totald,newValue,this.totala)
        this.chartData.labels=[]
        this.chartData.datasets[0].backgroundColor=[]
        this.chartData.datasets[0].borderColor=[]
        for (let i=0;i<this.totalf.length;i++){
          this.chartData.labels.push(`F${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(198,229,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(198,229,177)')
        }
        for (let i=0;i<this.totald.length;i++){
          this.chartData.labels.push(`D${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(249,249,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(249,249,177)')
        }
        for (let i=0;i<newValue.length;i++){
          this.chartData.labels.push(`O${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(157,191,229,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(157,191,229)')
        }
        for (let i=0;i<this.totala.length;i++){
          this.chartData.labels.push(`A${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(228,111,108,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(228,111,108)')
        }
        this.chartData.datasets[0].data = this.values
      },
      totald(newValue) {
        this.values=this.totalf.concat(newValue,this.totalo,this.totala)
        this.chartData.labels=[]
        this.chartData.datasets[0].backgroundColor=[]
        this.chartData.datasets[0].borderColor=[]
        for (let i=0;i<this.totalf.length;i++){
          this.chartData.labels.push(`F${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(198,229,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(198,229,177)')
        }
        for (let i=0;i<newValue.length;i++){
          this.chartData.labels.push(`D${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(249,249,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(249,249,177)')
        }
        for (let i=0;i<this.totalo.length;i++){
          this.chartData.labels.push(`O${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(157,191,229,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(157,191,229)')
        }
        for (let i=0;i<this.totala.length;i++){
          this.chartData.labels.push(`A${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(228,111,108,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(228,111,108)')
        }
        this.chartData.datasets[0].data = this.values
      },
      totala(newValue) {
        this.values=this.totalf.concat(this.totald,this.totalo,newValue)
        this.chartData.labels=[]
        this.chartData.datasets[0].backgroundColor=[]
        this.chartData.datasets[0].borderColor=[]
        for (let i=0;i<this.totalf.length;i++){
          this.chartData.labels.push(`F${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(198,229,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(198,229,177)')
        }
        for (let i=0;i<this.totald.length;i++){
          this.chartData.labels.push(`D${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(249,249,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(249,249,177)')
        }
        for (let i=0;i<this.totalo.length;i++){
          this.chartData.labels.push(`O${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(157,191,229,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(157,191,229)')
        }
        for (let i=0;i<newValue.length;i++){
          this.chartData.labels.push(`A${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(228,111,108,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(228,111,108)')
        }
        this.chartData.datasets[0].data = this.values
      },
  },
  mounted(){
      this.values=this.totalf.concat(this.totald,this.totalo,this.totala)
      this.chartData.labels=[]
      this.chartData.datasets[0].backgroundColor=[]
      this.chartData.datasets[0].borderColor=[]
        for (let i=0;i<this.totalf.length;i++){
          this.chartData.labels.push(`F${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(198,229,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(198,229,177)')
        }
        for (let i=0;i<this.totald.length;i++){
          this.chartData.labels.push(`D${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(249,249,177,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(249,249,177)')
        }
        for (let i=0;i<this.totalo.length;i++){
          this.chartData.labels.push(`O${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(157,191,229,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(157,191,229)')
        }
        for (let i=0;i<this.totala.length;i++){
          this.chartData.labels.push(`A${i+1}`)
          this.chartData.datasets[0].backgroundColor.push('rgba(228,111,108,1.000)')
          this.chartData.datasets[0].borderColor.push('rgb(228,111,108)')
        }
        this.chartData.datasets[0].data = this.values
  }
};
</script>