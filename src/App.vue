<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col cols="3">
            <Average
              @update-end-hour='updateEndHour'
              @update-end-min='updateEndMin'
              @update-start-hour='updateStartHour'
              @update-start-min='updateStartMin'
            />
            <v-btn
              block
              class="white--text"
              color='#2EA169'
              style="margin-left:5%;margin-top:10%;"
              v-on:click='graph'
            >Graph Avg</v-btn>
          </v-col>
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
                :bus='bus'
                :sliderValue='sliderValue'
                :endHour='endHour'
                :endMin='endMin'
                :startHour='startHour'
                :startMin='startMin'
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
import Vue from 'vue';

export default {
  name: 'App',
  components: {
    Average,
    Graph,
    Slider,
  },
  data () {
    return {
      bus: new Vue(),
      endHour: null,
      endMin: null,
      rowCount: 0,
      sliderValue: 1,
      startHour: null,
      startMin: null,
    }
  },
  methods: {
    graph() {
      this.bus.$emit('graph');
    },
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