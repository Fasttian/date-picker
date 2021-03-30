<template>
	<view class="uni-date">
		<view class="uni-date-editor--X" @click="show">
			<view v-if="!range" class="uni-date-x uni-date-single mt20">
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
				<input class="uni-date__input uni-date-range__input" type="text" :value="displayValue"
					:placeholder="startPlaceholder" />
				<slot>
					<view class="">{{rangeSeparator}}</view>
				</slot>
				<input class="uni-date__input uni-date-range__input" type="text" :value="displayValue"
					:placeholder="endPlaceholder" />
				<view class="uni-date__icon-clear" @click="clear">
					<uni-icons type="clear" color="#e1e1e1" size="14"></uni-icons>
				</view>
			</view>
		</view>

		<view ref="datePicker" v-if="popup" class="uni-date-picker__container" @click="show">
			<!-- <view class="uni-date-mask" @click="open"></view> -->
			<view v-if="!range" class="uni-date-single--x" :style="popover">
				<uni-calendar :selected="info.selected" :showMonth="false" @change="change"
					@monthSwitch="monthSwitch" />
			</view>

			<view v-else class="uni-date-range--x" :style="popover">
				<uni-calendar :selected="info.selected" :showMonth="false" @change="change" @monthSwitch="monthSwitch"
					style="padding-right: 16px;" />
				<uni-calendar :selected="info.selected" :showMonth="false" @change="change" @monthSwitch="monthSwitch"
					style="padding-left: 16px;border-left: 1px solid #F1F1F1;" />
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
				range: false,
				visible: false,
				popup: false,
				popover: null,
				displayValue: '',
				info: {
					lunar: true,
					range: true,
					insert: false,
					selected: []
				}
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
						this.range = false
					} else if (newVal === 'daterange') {
						this.range = true
					}
				}
			}
		},
		computed: {

		},
		methods: {

			show(event) {
				if (this.disabled || false) {
					return
				}
				// this.valueChangeSource = ''
				// console.log(11111, this.$refs.datePicker);
				// const $datePicker = this.$refs.datePicker
				// $picker.remove();
				// (document.querySelector('uni-app') || document.body).appendChild(this.$refs.datePicker)
				// this.$refs.datePicker.style.display = 'block'
				const dateEditor = uni.createSelectorQuery().in(this).select(".uni-date-editor--X")
				dateEditor.boundingClientRect(rect => {
					this.popover = {
						top: rect.top + rect.height + 50 + 'px',
						left: rect.left + 'px',
						// width: rect.width,
						// height: rect.height
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
				// 模拟动态打卡
				if (this.info.selected.length > 5) return
				this.info.selected.push({
					date: e.fulldate,
					info: '打卡'
				})
			},

			clear() {
				this.displayValue = ''
			},













			confirm(e) {
				console.log('confirm 返回:', e)
			},
			monthSwitch(e) {
				console.log('monthSwitchs 返回:', e)
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
