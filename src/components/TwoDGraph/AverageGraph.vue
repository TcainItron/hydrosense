<template>
	<v-app>
		<v-main>
			<v-container>
				<v-row>
						<TimeSelector
							:bus='bus'
              :minTime='this.rows.length ? this.rows[1][0].split(" ").join("T") : ""'
              :maxTime='this.rows.length ? this.rows[this.rows.length-1][0].split(" ").join("T") : ""'
						/>
				</v-row>
				<v-row>
						<Graph
							:bus='bus'
							:rows='rows'
							:sliderValue=null
						/>
				</v-row>
			</v-container>
		</v-main>
	</v-app>
</template>

<script>
	import TimeSelector from '../TimeSelector.vue';
	import Graph from './Graph';
	import Vue from 'vue';

	export default {
		name: 'App',
		props: ['rows'],
		components: {
			TimeSelector,
			Graph,
		},
		data () {
			return {
				bus: new Vue(),
				rowCount: 0,
			}
		},
	};
</script>