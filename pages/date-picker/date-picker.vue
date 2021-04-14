<template>
	<view style="padding: 20px; width: 350px; margin: 0 auto;">
		<view style="margin: 30px 0;">单选</view>
		<uni-date-picker v-model="single" />
		<view style="margin: 30px 0;">单选 无边框</view>
		<uni-date-picker v-model="single" :border="false" />
		<view style="margin: 30px 0;">范围选</view>
		<uni-date-picker v-model="range" type="daterange" rangeSeparator="至" />
		<view style="margin: 30px 0;">范围选 disabled</view>
		<uni-date-picker v-model="range" type="daterange" rangeSeparator="至" :disabled="true" />
	</view>
</template>

<script>
	export  default {
		data() {
			return {
				single: '2010-02-3',
				range: ['2020-10-8',  '2021-4-20']
			}
		},
		
		watch: {
			single(newval) {
				console.log('单选:', this.single);
			},
			range(newval) {
				console.log('范围选:', this.range);
			}
		}
	}
</script>