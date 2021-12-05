<template>
  <v-tabs background-color="cyan" dark>
    <v-tab>2D Grpah</v-tab>
    <v-tab>3D Graph</v-tab>
  </v-tabs>
</template>

<!-- <script>
	export default {
		name: 'Average',
		data () {
			return {
				endHour: null,
				endMin: null,
				hours: [...Array(24).keys()].map(x => x.toString().padStart(2, '0')),
				mins: [...Array(60).keys()].map(x => x.toString().padStart(2, '0')),
				rowCount: 0,
				sliderValue: 1,
				startHour: null,
				startMin: null,
			}
		},
		methods: {
			updateEndHour() {
				this.$emit('update-end-hour', this.endHour);
			},
			updateEndMin() {
				this.$emit('update-end-min', this.endMin);
			},
			updateStartHour() {
				this.$emit('update-start-hour', this.startHour);
			},
			updateStartMin() {
				this.$emit('update-start-min', this.startMin);
			},
		}
	};
</script>

<style>
	.avgs {
		margin-top: 10%;
    width: 100%;
	}
</style> -->