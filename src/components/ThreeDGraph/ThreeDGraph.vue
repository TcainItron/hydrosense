<template>
<body>
	<div class="small">
		<div :id='graphID'><!-- Plotly chart will be drawn inside this DIV --></div>
	</div>
</body>
</template>

<script>
	import Plotly from 'plotly.js';

	export default {
		name: 'Topographical',
		props: ['bus', 'graphID', 'render', 'rows'],
		watch: {
			rows: function(newRows) {
				if (this.render) this.updateGraphData(newRows.slice(1));
			},
		},
		mounted() {
			if (this.render && this.rows.length>0) this.updateGraphData(this.rows.slice(1));
			this.bus.$on('graph', this.graphAvg);
		}, 
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
			}
		},
		methods: {
			updateGraphData(updatedRows) {
				this.graphData = [{
					x: this.rows[0].slice(1),
					y: updatedRows.map(row => (row[0].split(" "))[1].split(".")[0]),
					z: updatedRows.map(row => row.slice(1)),
					colorscale: [
						['0.0', 'rgb(249,242,255'],
						['0.01', 'rgb(236,229,255'],
						['0.02', 'rgb(223,216,255'],
						['0.03', 'rgb(211,204,255'],
						['0.04', 'rgb(198,191,255'],
						['0.05', 'rgb(185,178,255'],
						['0.1', 'rgb(172,165,255'],
						['0.15', 'rgb(160,153,255'],
						['0.2', 'rgb(147,140,255'],
						['0.25', 'rgb(134 127,255'],
						['0.3', 'rgb(121 114,255'],
						['0.35', 'rgb(108 101,255'],
						['0.4', 'rgb(96 89,255'],
						['0.45', 'rgb(83 76,247'],
						['0.5', 'rgb(70 63,234'],
						['0.55', 'rgb(57 50,221'],
						['0.6', 'rgb(45 38,209'],
						['0.65', 'rgb(32,25,196)'],
						['0.7', 'rgb(19,12,183)'],
						['0.75', 'rgb(6,0,170)'],
						['0.8', 'rgb(0,0,158)'],
						['0.85', 'rgb(0,0,145)'],
						['0.9', 'rgb(0,0,132)'],
						['0.95', 'rgb(0,0,119)'],
						['1.0', 'rgb(0,0,107)'],
					],
					type: 'surface'
				}];
				Plotly.newPlot(this.graphID, this.graphData, this.options);
			},
			graphAvg(times) {
				let rows = this.findRowsToBeAveraged(times[0], times[1]);
				if (rows) {
					this.updateGraphData(rows);
				} else {
					alert("Could not find start and stop times in data set");
				}
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
