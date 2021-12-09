<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
            <ThreeDGraph
				:render='true'
                :bus='bus'
				:rows='rows'
				:graphID='all'
            />
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
	import ThreeDGraph from './ThreeDGraph.vue';
	import Vue from 'vue';

	export default {
		name: 'App',
		props: ['rows'],
		components: {
			ThreeDGraph,
		},
		data () {
			return {
				all: 'all',
				bus: new Vue(),
			}
		},
	};
</script>