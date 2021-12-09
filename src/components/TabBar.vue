<template>
	<v-tabs background-color="#005b77" dark>
		<v-tab
			v-for="tab in tabs"
			:key="tab.title"
		>
		{{ tab.title }}
		</v-tab>

		<div style="margin-left:auto;align-items:center;">
			<v-btn
				color="#005b77"
				depressed
				@click="uploadFile"
			>
				<v-icon>mdi-file-upload-outline</v-icon>
				Upload Dataset
			</v-btn>
			<input
				ref="file"
				class="d-none"
				type="file"
				accept=".csv"
				@change="loadNewDataset"
			>
			</div>

		<v-tab-item
			v-for="tab in tabs"
			:key="tab.title"
		>
				<v-tabs background-color="#F0F0F0" color="#005b77">
					<v-tab
						v-for="innerTab in tab.innerTabs"
						:key="innerTab.title"
					>
						{{ innerTab.title }}
					</v-tab>
					<v-tab-item
						v-for="innerTab in tab.innerTabs"
						:key="innerTab.title"
					>
						<component :is="innerTab.component" :rows="rows"/>
					</v-tab-item>
				</v-tabs>
		</v-tab-item>
	</v-tabs>
</template>

<script>
	import Specified3DGraph from './ThreeDGraph/SpecifiedDataGraph.vue';
	import AllData3DGraph from './ThreeDGraph/AllDataGraph.vue';
	import AverageGraph from './TwoDGraph/AverageGraph.vue';
	import Papa from 'papaparse';
	import SliderGraph from './TwoDGraph/SliderGraph.vue';

	export default {
		name: 'TabBar',
		components: {
			'avg-graph': AverageGraph,
			'slider-graph': SliderGraph,
			'specified-threeD-graph': Specified3DGraph,
			'all-data-threeD-graph': AllData3DGraph,
		},
		data() {
			return {
				rows: [],
				tabs: [
					{
						title: '2D Graph',
						innerTabs: [
							{
								title: 'Graph An Avg',
								component: 'avg-graph',
							},
							{
								title: 'Slide Through Data',
								component: 'slider-graph'
							}
						]
					},
					{
						title: '3D Graph',
						innerTabs: [
							{
								title: 'Graph Specified Times',
								component: 'specified-threeD-graph'
							},
							{
								title: 'Graph All Data',
								component: 'all-data-threeD-graph'
							},
						]
					},
				],
			}
		},
		methods: {
			uploadFile() {
				this.$refs.file.click();
			},
			async loadNewDataset() {
				this.rows = await this.parseCSV(this.$refs.file.files[0]);
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
	};
</script>

<style>
	.avgs {
		margin-top: 10%;
	width: 100%;
	}
</style>