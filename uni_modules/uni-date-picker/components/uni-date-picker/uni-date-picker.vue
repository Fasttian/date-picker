<template>
	<view class="uni-date">
		<view v-if="!range" class="uni-date-x uni-date-single mt20">
			<view class="uni-date__icon-logo"><uni-icons type="list" color="#666"></uni-icons></view>
			<input class="uni-date__input" type="text" :value="displayValue" :placeholder="placeholder" />
			<view class="uni-date__icon-clear"><uni-icons type="clear" color="#e1e1e1" size="14"></uni-icons></view>
		</view>
		<view  class="uni-date-x uni-date-range mt20">
			<view class="uni-date__icon-logo"><uni-icons type="list" color="#666"></uni-icons></view>
			<input class="uni-date__input uni-date-range__input" type="text" :value="displayValue" :placeholder="placeholder" />
			<slot>
				<view class="">{{rangeSeparator}}</view>
			</slot>
			<input class="uni-date__input uni-date-range__input" type="text" :value="displayValue" :placeholder="placeholder" />
			<view class="uni-date__icon-clear"><uni-icons type="clear" color="#e1e1e1" size="14"></uni-icons></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				range: false
			}
		},
		props: {
			placeholder: {
				type: String,
				default: ''
			},
			rangeSeparator: {
				default: '-'
			}
		},
		computed: {
			displayValue() {
				return 123
			}
		},
		methods: {

		}
	}
</script>

<style>
	.uni-date-x {
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 0 10px;
		border: 1px solid #dcdfe6;
		border-radius: 4px;
		background-color: #fff;
		color: #666;
	}
	
	.uni-date__input {
		height: 40px;
		width: 100%;
		padding: 0 8px;
		line-height: 40px;
	}
	.uni-date-range__input {
		text-align: center;
	}
	
	.mt20 {
		margin-top: 20px;
	}
</style>
