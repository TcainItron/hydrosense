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
    props: ['bus', 'endHour', 'endMin', 'startHour', 'startMin', 'sliderValue'],
    watch: {
      sliderValue: function(newVal) {
        this.updateGraphData(this.rows[newVal]);
      },
    },
    mounted() {
      this.bus.$on('graph', this.graphAvg);
    }, 
    data () {
      return {
        graphData: {},
        options: {
          animation: {
            duration: 500,
          },
          legend: {
            display: false,
          },
          responsive: true,
          maintainAspectRatio: false,
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
      graphAvg() {
        if (!(this.endHour && this.endMin && this.startHour && this.startMin)) {
          alert('Please select a start and end time');
          return;
        }
        var start = (parseInt(this.startHour) * 60) + parseInt(this.startMin);
        var end = (parseInt(this.endHour) * 60) + parseInt(this.endMin);
        this.updateGraphData(this.findAvg(start, end));
      },
      findAvg(start, end) {
        var rowsToBeAveraged = this.rows.slice(start+1, end+1);
        var results = [];
        for (var i = 0; i < this.rows[0].length; i++) {
          results.push(0);
          for (const row of rowsToBeAveraged) {
            results[i] = results[i] + parseInt(row[i]);
          }
          results[i] = results[i] / rowsToBeAveraged.length;
        }
        return results
      },
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
        this.$emit('update-row-count', this.rows.length - 1);
      },
      parseCSV(file) {
        return new Promise( (resolve) => {
          Papa.parse(file, {
            complete: (results) => {
              resolve(results.data);
            }
          });
        })
      },
    }
  }
</script>

<style>
  .small {
    width: 80%;
    height: 100%;
    margin: 5% auto;
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