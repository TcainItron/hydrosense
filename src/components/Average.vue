<template>
	<v-col class='avgs' cols="3">
		<h2 style='margin-left:20%;'>Graph an Average</h2>
		<v-row>
			<div style='margin-top:21%;margin-right:5%;width:15%;text-align:right;'>From:</div>
			<v-select v-model='startHour' style='width:5%;margin-top:15%;' label="HH" :items="hours"></v-select>

			<div style="margin-top:20%;margin-left:2%;margin-right:2%;"> : </div>
			<v-select v-model='startMin' style='width:5%;margin-top:15%;' label="mm" :items="mins"></v-select>
		</v-row>
		<v-row>
			<div style='margin-top:15%;margin-right:5%;width:15%;text-align:right;'>  To:</div>
			<v-select v-model='endHour' style='width:5%;margin-top:10%;' label="HH" :items="hours"></v-select>

			<div style="margin-top:15%;margin-left:2%;margin-right:2%;"> : </div>
			<v-select v-model='endMin' style='width:5%;margin-top:10%;' label="mm" :items="mins"></v-select>
		</v-row>
	</v-col>
</template>

<script>
	export default {
		name: 'Average',
		data () {
			return {
				endHour: null,
				endMin: null,
				hours: [...Array(24).keys()].map(x => x.toString().padStart(2, '0')),
				mins: [...Array(60).keys()].map(x => x.toString().padStart(2, '0')),
				rowCount: 0,
				sliderValue: 1,
				startHour: null,
				startMin: null,
			}
		},
	};
</script>

<style>
	.avgs {
		margin-top: 10%;
    width: 100%;
	}
</style>