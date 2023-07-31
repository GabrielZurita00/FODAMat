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
    components: {
      BarChart: Bar,
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
          labels: ['F1', 'F2', 'F3', 4, 5],
          datasets: [
            {
              label: 'test data',
              data: [20, 30, 40, 50, 60],
            },
          ],
        },
      };
    },
  };
  </script>
  