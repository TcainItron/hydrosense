<template>
  <v-app>
    <v-main>
      <v-container>
        <TabBar/>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
	import TabBar from './components/TabBar.vue';

	export default {
		name: 'App',
		components: {
			TabBar,
		},
	};
</script>