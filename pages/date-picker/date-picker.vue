<template>
	<view style="padding: 20px; width: 350px;">
		<view>单选</view>
		<uni-date-picker v-model="single" />
		<view style="margin-top: 30px;">范围选</view>
		<uni-date-picker v-model="range" type="daterange" rangeSeparator="至" />
	</view>
</template>

<script>
	export  default {
		data() {
			return {
				single: '2010-02-3',
				range: ['2020-10-1',  '2021-4-1']
			}
		},
		
		watch: {
			single(newval) {
				console.log(33333333, this.single);
			}
		}
	}
</script>