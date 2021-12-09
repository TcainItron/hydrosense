<template>
		<v-slider
			class="slider"
			:color="color"
			:max="rowCount"
			:min="min"
			v-model='sliderValue'
			@input='updateSliderValue'
		/>
</template>

<script>
	export default {
		name: 'Slider',
		props: ['rowCount'],
		data () {
			return {
				sliderValue: '',
				color: '#005b77',
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
		margin-left: 20%;
		margin-right: 20%;
		margin-top: 10%;
		width: 100%;
	}
</style>