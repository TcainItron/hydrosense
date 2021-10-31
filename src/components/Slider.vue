<template>
		<v-slider
			class="slider"
      :color="color"
			:max="max"
			:min="min"
			v-model='sliderValue'
			@input='log'
    />
</template>

<script>
	export default {
		name: 'Slider',
		data () {
			return {
				sliderValue: '',
				color: '#2EA169',
				max: 5,
				min: 1,
			}
		},
		methods: {
			log() {
				this.$emit('update-slider-value', this.sliderValue)
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