<template>
  <div class="small">
    <LineChart :chartData="graphData" :options="options"></LineChart>
  </div>
</template>

<script>
  import LineChart from './LineChart.js';

  export default {
    name: 'Graph',
    components: {
      LineChart
    },
    props: ['bus', 'rows', 'sliderValue'],
    watch: {
      sliderValue: function(newVal) {
        if (this.rows.length>0) this.updateGraphData(this.rows[newVal]);
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
                suggestedMax: 3000,
                stepSize: 250,
              }
            }]
          },
        },
      }
    },
    methods: {
      //Maybe move this to Avg.vue
      graphAvg(times) {
        let rows = this.findRowsToBeAveraged(times[0], times[1]);
        if (rows) {
          this.updateGraphData(this.findAvg(rows));
        } else {
          alert("Could not find start and stop times in data set");
        }
      },
      findAvg(rowsToBeAveraged) {
        var results = [0];
        for (var i = 1; i < this.rows[0].length; i++) {
          // Pushing 0 into results before indexing to avoid index errors. The 0 immediatly gets replaced
          results.push(0);
          for (const row of rowsToBeAveraged) {
            results[i] = results[i] + parseInt(row[i]);
          }
          results[i] = results[i] / rowsToBeAveraged.length;
        }
        return results.slice(1);
      },
      findRowsToBeAveraged(start, end) {
        let startIndex = 0;
        let endIndex = 0;
        for (const row in this.rows) {
          const startPattern = new RegExp("\\b"+start+"\\b:.*");
          if (startPattern.test(this.rows[row][0])) {
            startIndex = row;
          }
          const endPattern = new RegExp("\\b"+end+"\\b:.*");
          if (endPattern.test(this.rows[row][0])) {
            endIndex = row;
          }
        }
        if (startIndex && endIndex) {
          return this.rows.slice(startIndex, endIndex+1);
        }
        return false;
      },
      updateGraphData(dataPoints) {
        this.graphData = {
          labels: this.rows[0].slice(1),
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