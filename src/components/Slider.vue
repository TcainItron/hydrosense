<template>
		<v-slider
			class="slider"
			:color="color"
			:max="max"
			:min="min"
			v-model='sliderValue'
			@input='updateSliderValue'
	/>
</template>

<script>
	export default {
		name: 'Slider',
		props: ['rowCount'],
		watch: {
			rowCount: function(newVal) {
				this.max = newVal;
			},
		},
		data () {
			return {
				sliderValue: '',
				color: '#2EA169',
				max: 1,
				min: 1,
			}
		},
		methods: {
			updateSliderValue() {
				this.$emit('update-slider-value', this.sliderValue);
			},
		}
	}
</script>

<style>
	.slider {
		margin-left: 30%;
		margin-right: 30%;
		margin-top: 10%;
	}
</style>