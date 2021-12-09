<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
            <Slider
                @update-slider-value='updateSliderValue'
                :rowCount="rows.length-1"
            />
        </v-row>
        <v-row>
            <Graph
                :bus='bus'
				:rows='rows'
                :sliderValue='sliderValue'
            />
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
	import Graph from './Graph';
	import Slider from './Slider.vue';
	import Vue from 'vue';

	export default {
		name: 'App',
		props: ['rows'],
		components: {
			Graph,
			Slider,
		},
		data () {
			return {
				bus: new Vue(),
				sliderValue: 1,
			}
		},
		methods: {
			graph() {
				this.bus.$emit('graph');
			},
			updateSliderValue(value) {
				this.sliderValue = value;
			},
		}
	};
</script>