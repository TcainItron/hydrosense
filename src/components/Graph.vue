<template>
  <div class="small">
    <LineChart :chartData="graphData" :options="options"></LineChart>
    <label class="file-select">
      <div class="select-button">
        <span>Load a Dataset:</span>
      </div>
      <input type="file" @change="updateGraphData" ref='file'/>
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
    data () {
      return {
        graphData: {},
        options: {
          legend: {
            display: false,
          },
        },
      }
    },
    methods: {
      async updateGraphData() {
        let rows = await this.parseCSV();
        this.graphData = {
          labels: rows[0],
          datasets: rows.slice(1).map(row => new Object(
            {
              fill: false,
              borderColor: '#f87979',
              data: row,
              tension: 0.1
            }
          )),
        }
      },
      parseCSV() {
        return new Promise( (resolve) => {
          Papa.parse(this.$refs.file.files[0], {
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
    margin:  150px auto;
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