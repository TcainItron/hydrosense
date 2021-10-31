<template>
  <v-app>
    <v-main>
      <v-container>
        <v-col>
          <v-row>
            <Slider
              @update-slider-value='updateSliderValue'
            />
          </v-row>
          <v-row>
            <Graph
              :sliderValue='sliderValue'
            />
          </v-row>
        </v-col>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Graph from './components/Graph';
import Slider from './components/Slider.vue';

export default {
  name: 'App',
  components: {
    Graph,
    Slider,
  },
  data () {
    return {
      sliderValue: 1,
    }
  },
  methods: {
    updateSliderValue(value) {
      this.sliderValue = value;
    }
  }
};
</script>
