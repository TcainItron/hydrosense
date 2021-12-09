<template>
	<div class='avgs'>
			<v-row>
				<v-col>
					<Datetime :min-datetime="minTime" :max-datetime="maxTime" class='theme-itron' use12-hour value-zone="local" input-id="startDate" v-model="startTime" type="datetime" style="border-bottom: 1px solid #A9A9A9;">
						<label class="grey--text mb-2" for="startDate" slot="before">
							Start Time
							<v-icon v-show="!startTime">mdi-chevron-down</v-icon>
						</label>
					</Datetime>
				</v-col>
				<v-col>
					<Datetime :min-datetime="minTime" :max-datetime="maxTime" class='theme-itron' use12-hour value-zone="local" input-id="stopDate" v-model="stopTime" type="datetime" style="border-bottom: 1px solid #A9A9A9;">
						<label class="grey--text mb-2" for="stopDate" slot="before">
							Stop Time
							<v-icon v-show="!stopTime">mdi-chevron-down</v-icon>
						</label>
					</Datetime>
				</v-col>
				<v-col>
					<v-btn
						block
						class="white--text"
						color='#005b77'
						style="min-width:50%!important;max-width:50%;"
						v-on:click='graph'
					>Graph Avg</v-btn>
				</v-col>
			</v-row>
	</div>
</template>

<script>
	import { Datetime } from 'vue-datetime';
	import 'vue-datetime/dist/vue-datetime.css'
	export default {
		name: 'Average',
		components: {
			Datetime,
		},
		props: ['bus', 'maxTime', 'minTime'],
		data () {
			return {
				rowCount: 0,
				sliderValue: 1,
				startTime: null,
				stopTime: null,
			}
		},
		methods: {
			graph() {
				let start = this.format(this.startTime);
				let stop = this.format(this.stopTime);
				this.bus.$emit('graph', [start.date+' '+start.time, stop.date+' '+stop.time]);
			},
			format(timestamp) {
				return {
					'date': timestamp.split("T")[0], 
					'time': timestamp.split("T")[1].slice(0,5)
				}
			}
		}
	};
</script>

<style>
	.avgs {
		margin-left: 18%;
		width: 100%;
	}
.theme-itron .vdatetime-popup__header,
.theme-itron .vdatetime-calendar__month__day--selected > span > span,
.theme-itron .vdatetime-calendar__month__day--selected:hover > span > span {
	background: #005b77;
}

.theme-itron .vdatetime-year-picker__item--selected,
.theme-itron .vdatetime-time-picker__item--selected,
.theme-itron .vdatetime-popup__actions__button {
	color: #005b77;
}
</style>