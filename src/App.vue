<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <Average
            @update-end-hour='updateEndHour'
            @update-end-min='updateEndMin'
            @update-start-hour='updateStartHour'
            @update-start-min='updateStartMin'
          />
          <v-col cols="9">
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
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Average from './components/Average.vue';
import Graph from './components/Graph';
import Slider from './components/Slider.vue';

export default {
  name: 'App',
  components: {
    Average,
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
    },
    updateEndHour(hour) {
      this.endHour = hour;
    },
    updateEndMin(min) {
      this.endMin = min;
    },
    updateStartHour(hour) {
      this.startHour = hour;
    },
    updateStartMin(min) {
      this.startMin = min;
    },
  }
};
</script>