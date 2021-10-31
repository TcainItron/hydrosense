<template>
  <v-app>
    <v-main>
      <v-container>
        <v-col>
          <v-row>
            <Slider
              @update-slider-value='updateSliderValue'
              :rowCount="rowCount"
            />
          </v-row>
          <v-row>
            <Graph
              @update-row-count='updateRowCount'
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
      rowCount: 0,
      sliderValue: 1,
    }
  },
  methods: {
    updateSliderValue(value) {
      this.sliderValue = value;
    },
    updateRowCount(count) {
      this.rowCount = count;
    }
  }
};
</script>
