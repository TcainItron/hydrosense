<template>
<body>
  <div class="small">
    <div id='topo'><!-- Plotly chart will be drawn inside this DIV --></div>
    <label class="file-select">
      <div class="select-button">
        <span>Load a Dataset:</span>
      </div>
      <input type="file" @change="loadNewDataset" ref='file'/>
    </label>
  </div>
</body>
</template>

<script>
  import Papa from 'papaparse';
  import Plotly from 'plotly.js';

  export default {
    name: 'Topographical',

    data () {
      return {
        graphData: [],
        options: {
            title: 'Water Usage',
            autosize: false,
            width: 1000,
            height: 1000,
            scene: {
                xaxis: {title: 'Frequency'},
                yaxis: {title: 'Time'},
                zaxis: {title: 'Amplitude'}
            },
            margin: {
                l: 65,
                r: 50,
                b: 65,
                t: 90,
            }
        },
        rows: [],
      }
    },
    methods: {
      updateGraphData() {
        this.graphData = [{
           x: this.rows[0].slice(1),
           y: this.rows.slice(1).map(row => (row[0].split("  "))[1]),
           z: this.rows.slice(1).map(row => row.slice(1)),
        colorscale: [
            ['0.0', 'rgb(165,0,38)'],
            ['0.01', 'rgb(116,173,209)'],
            ['0.02', 'rgb(69,117,180)'],
            ['0.03', 'rgb(69,117,180)'],
            ['0.04', 'rgb(49,54,149)'],
            ['0.05', 'rgb(215,48,39)'],
            ['0.1', 'rgb(244,109,67)'],
            ['0.15', 'rgb(253,174,97)'],
            ['0.2', 'rgb(254,224,144)'],
            ['0.25', 'rgb(224,243,248)'],
            ['0.3', 'rgb(171,217,233)'],
            ['0.35', 'rgb(116,173,209)'],
            ['0.4', 'rgb(69,117,180)'],
            ['0.45', 'rgb(215,48,39)'],
            ['0.5', 'rgb(244,109,67)'],
            ['0.55', 'rgb(253,174,97)'],
            ['0.6', 'rgb(254,224,144)'],
            ['0.65', 'rgb(224,243,248)'],
            ['0.7', 'rgb(171,217,233)'],
            ['0.75', 'rgb(116,173,209)'],
            ['0.8', 'rgb(69,117,180)'],
            ['0.85', 'rgb(116,173,209)'],
            ['0.9', 'rgb(69,117,180)'],
            ['0.95', 'rgb(69,117,180)'],
            ['1.0', 'rgb(49,54,149)']
        ],
           type: 'surface'
        }];
        Plotly.newPlot('topo', this.graphData, this.options);
      },
      async loadNewDataset() {
        this.rows = await this.parseCSV(this.$refs.file.files[0]);
        this.updateGraphData();
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
    },
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
