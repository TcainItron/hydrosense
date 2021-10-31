<template>
  <div class="small">
    <LineChart :chartData="graphData" :options="options"></LineChart>
    <label class="file-select">
      <div class="select-button">
        <span>Load a Dataset:</span>
      </div>
      <input type="file" @change="loadNewDataset" ref='file'/>
  </label>
  </div>
</template>

<script>
  import LineChart from './LineChart.js';
  import Papa from 'papaparse';

  export default {
    name: 'Graph',
    components: {
      LineChart
    },
    props: ['sliderValue'],
    watch: {
      sliderValue: function(newVal) {
        this.updateGraphData(this.rows[newVal]);
      },
    },
    data () {
      return {
        graphData: {},
        options: {
          legend: {
            display: false,
          },
          scales: {
            yAxes: [{
              ticks: {
                min: 0,
                max: 3000,
                stepSize: 250,
              }
            }]
          },
        },
        rows: [],
      }
    },
    methods: {
      updateGraphData(dataPoints) {
        this.graphData = {
          labels: this.rows[0],
          datasets: [
            {
              fill: false,
              borderColor: '#f87979',
              data: dataPoints,
              tension: 0.1
            }
          ],
        }
      },
      async loadNewDataset() {
        this.rows = await this.parseCSV(this.$refs.file.files[0]);
      },
      parseCSV(file) {
        return new Promise( (resolve) => {
          Papa.parse(file, {
            complete: (results) => {
              resolve(results.data)
            }
          });
        })
      },
    }
  }
</script>

<style>
  .small {
    max-width: 600px;
    margin:  50px auto;
  }
  .file-select > .select-button {
    padding: 1rem;

    color: white;
    background-color: #2EA169;

    border-radius: .3rem;

    text-align: center;
    font-weight: bold;
  }

  /* Don't forget to hide the original file input! */
  .file-select > input[type="file"] {
    display: none;
  }
</style>