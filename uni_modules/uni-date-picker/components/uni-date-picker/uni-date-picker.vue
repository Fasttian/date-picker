<template>
	<view class="uni-date">
		<view class="uni-date-editor--X" @click="show">
			<view v-if="!isRange" class="uni-date-x uni-date-single mt20">
				<view class="uni-date__icon-logo">
					<uni-icons type="list" color="#666"></uni-icons>
				</view>
				<input class="uni-date__input" type="text" :value="displayValue" :placeholder="placeholder" />
				<view class="uni-date__icon-clear" @click="clear">
					<uni-icons type="clear" color="#e1e1e1" size="14"></uni-icons>
				</view>
			</view>
			<view v-else class="uni-date-x uni-date-range mt20">
				<view class="uni-date__icon-logo">
					<uni-icons type="list" color="#666"></uni-icons>
				</view>
				<input class="uni-date__input uni-date-range__input" type="text" :value="range.startVal"
					:placeholder="startPlaceholder" />
				<slot>
					<view class="">{{rangeSeparator}}</view>
				</slot>
				<input class="uni-date__input uni-date-range__input" type="text" :value="range.endVal"
					:placeholder="endPlaceholder" />
				<view class="uni-date__icon-clear" @click="clear">
					<uni-icons type="clear" color="#e1e1e1" size="14"></uni-icons>
				</view>
			</view>
		</view>

		<view ref="datePicker" v-show="popup" class="uni-date-picker__container">
			<!-- <view class="uni-date-mask" @click="open"></view> -->
			<view v-if="!isRange" class="uni-date-single--x" :style="popover">
				<uni-calendar :showMonth="false" @change="change" @monthSwitch="monthSwitch" />
			</view>

			<view v-else class="uni-date-range--x" :style="popover">
				<uni-calendar ref="left" :showMonth="false" :range="true" @change="startChange" :pleStatus="endMultipleStatus"
					@monthSwitch="leftMonthSwitch" style="padding-right: 16px;" />
				<uni-calendar ref="right" :showMonth="false" :range="true" @change="endChange" :pleStatus="startMultipleStatus"
					@monthSwitch="rightMonthSwitch" style="padding-left: 16px;border-left: 1px solid #F1F1F1;" />
			</view>
		</view>
	</view>
</template>



<script>
	/**
	 * 获取任意时间
	 */

	export default {
		data() {
			return {
				isRange: false,
				range: {
					startVal: '',
					endVal: ''
				},
				startMultipleStatus: {
					before: '',
					after: '',
					data: [],
					fulldate: ''
				},
				endMultipleStatus: {
					before: '',
					after: '',
					data: [],
					fulldate: ''
				},
				visible: false,
				popup: false,
				popover: null,
				displayValue: ''
			}
		},
		props: {
			type: {
				type: String,
				default: 'date'
			},
			value: {
				type: [String, Number],
				default: ''
			},
			placeholder: {
				type: String,
				default: '选择日期'
			},
			startPlaceholder: {
				type: String,
				default: '起始日期'
			},
			endPlaceholder: {
				type: String,
				default: '结束日期'
			},
			rangeSeparator: {
				default: '-'
			}
		},
		watch: {
			type: {
				immediate: true,
				handler(newVal, oldVal) {
					if (newVal === 'date') {
						this.isRange = false
					} else if (newVal === 'daterange') {
						this.isRange = true
					}
				}
			}
		},
		computed: {

		},
		mounted() {
			if(this.isRange) {
				this.$refs.right.next()
			}
		},
		methods: {
			getRef() {
				console.log(66666666, this.$refs.left);
				this.$refs.left.pre()
			},
			show(event) {
				if (this.disabled || false) {
					return
				}
				const dateEditor = uni.createSelectorQuery().in(this).select(".uni-date-editor--X")
				dateEditor.boundingClientRect(rect => {
					this.popover = {
						top: rect.top + rect.height + 50 + 'px',
						left: rect.left + 'px',
					}
				}).exec()
				setTimeout(() => {
					this.popup = !this.popup
					// this.visible = true
				}, 20)
			},

			change(e) {
				// console.log('change 返回:', e)
				this.displayValue = e.fulldate
			},

			startChange(e) {
				// console.log('change start 返回:', e)
				this.range.startVal = e.fulldate
				const obj = {
					before: e.range.before,
					after: e.range.after,
					data: e.range.data,
					fulldate: e.fulldate
				}
				this.startMultipleStatus = Object.assign({}, this.startMultipleStatus, obj)
				console.log('startMultipleStatus 返回:', this.startMultipleStatus)
			},

			endChange(e) {
				// console.log('change end 返回:', e)
				this.range.endVal = e.fulldate
				const obj = {
					before: e.range.before,
					after: e.range.after,
					data: e.range.data,
					fulldate: e.fulldate
				}
				this.endMultipleStatus = Object.assign({}, this.endMultipleStatus, obj)
				console.log('endMultipleStatus 返回:', this.endMultipleStatus)
			},


			clear() {
				this.displayValue = ''
			},










			confirm(e) {
				console.log('confirm 返回:', e)
			},
			leftMonthSwitch(e) {
				console.log('leftMonthSwitch 返回:', e)
			},
			rightMonthSwitch(e) {
				console.log('rightMonthSwitch 返回:', e)
			}
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
		font-size: 14px;
	}

	.uni-date__input {
		height: 40px;
		width: 100%;
		padding: 0 8px;
		line-height: 40px;
		font-size: 14px;
	}

	.uni-date-range__input {
		text-align: center;
	}

	.uni-date-picker__container {
		/* display: none; */
		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
		box-sizing: border-box;
		z-index: 996;
		font-size: 14px;
	}

	.uni-date-mask {
		position: fixed;
		bottom: 0px;
		top: 0px;
		left: 0px;
		right: 0px;
		background-color: rgba(0, 0, 0, 0.4);
		transition-duration: 0.3s;
		z-index: 996;
	}

	.uni-date-single--x {
		position: absolute;
		top: 0;
		left: 0;
		z-index: 999;
		width: 375px;
		border: 1px solid #F1F1F1;
	}

	.uni-date-range--x {
		display: flex;
		position: absolute;
		top: 0;
		left: 0;
		z-index: 999;
		width: 733px;
		border: 1px solid #F1F1F1;
		border-radius: 4px;
	}

	.mt20 {
		margin-top: 20px;
	}
</style>
