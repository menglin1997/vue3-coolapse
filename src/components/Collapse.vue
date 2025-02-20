<template>
	<section
		class="collapse"
		ref="collapse"
		:style="{ height: containerH + 'px', 'padding-right': (hideControll ? 0 : 50) + 'px' }"
	>
		<div class="collapse-content" ref="content">
			<slot></slot>
		</div>
		<span v-if="!hideControll" v-show="controllVisible" class="collapse-controll" @click="handleControll">
			<i
				v-show="!hideControllIcon"
				class="collapse-controll-icon"
				:class="isShow ? 'el-icon-arrow-up' : 'el-icon-arrow-down'"
			></i>
			{{ isShow ? foldText : unfoldText }}
		</span>
	</section>
</template>

<script setup name="Collapse">
	import { ref, watch, onMounted, onUnmounted, nextTick } from 'vue'
	const props = defineProps({
		modelValue: Boolean,

		// 默认显示的内容高度
		defaultHeight: {
			type: Number,
			default: 100
		},

		// 是否隐藏内置控制按钮
		hideControll: Boolean,

		// 是否隐藏内置控制按钮icon
		hideControllIcon: Boolean,

		foldText: {
			type: String,
			default: '收起'
		},

		unfoldText: {
			type: String,
			default: '展开'
		}
	})

	const isShow = ref(false)
	const controllVisible = ref(true)
	const containerH = ref(props.defaultHeight)
	const contentH = ref(0)
	const DOMWatcher = ref(null)

	watch(
		() => props.modelValue,
		(v) => {
			if (v) {
				unfold()
			} else {
				fold()
			}
		}
	)

	onMounted(() => {
		nextTick(() => {
			init()
			_watch()
			if (props.modelValue) {
				unfold()
			}
		})
	})
	onUnmounted(() => {
		DOMWatcher.value.disconnect()
	})
	const content = ref()
	const init = () => {
		console.log(content.vlaue.offsetHeight, 'content.vlaue')
		contentH.value = content.vlaue.offsetHeight
		/**
		 * 当内容变化的同时也是展开状态的话，那么需要更新容器高度
		 * 1. 当最新的内容高度大于默认高度时，则把容器高度更新最新内容高度一致即可
		 * 2. 反之，把容器高度设到最小（即默认高度），同时设为收起状态
		 */
		if (isShow.value) {
			if (contentH.value > props.defaultHeight) {
				containerH.value = contentH.value
			} else {
				containerH.value = props.defaultHeight
				isShow.value = false
			}
		}
		controllVisible.value = contentH.value > props.defaultHeight // 控制按钮的显示隐藏
	}
	const emits = defineEmits(['change'])
	watch(
		() => isShow.value,
		(v) => {
			emits('change', v)
		}
	)

	const _watch = () => {
		this.DOMWatcher = new MutationObserver(() => {
			// 监测到DOM变化，重新计算高度
			refresh()
		})
		DOMWatcher.value.observe(content.value, {
			childList: true,
			attributes: true,
			characterData: true,
			subtree: true
		})
	}
	const refresh = () => {
		nextTick(() => {
			init()
		})
	}

	const handleControll = () => {
		let handle = isShow.value ? fold : unfold
		handle()
	}
	// 收起
	const fold = () => {
		containerH.value = props.defaultHeight
		nextTick(() => {
			isShow.value = false
		})
	}
	// 展开
	const unfold = () => {
		const contentH = content.value.offsetHeight
		console.log(contentH, props.defaultHeight, 'content.value')

		if (contentH > props.defaultHeight) {
			containerH.value = contentH
		}
		nextTick(() => {
			isShow.value = true
		})
	}
</script>

<style scoped>
	.collapse {
		width: 100%;
		position: relative;
		overflow: hidden;
		transition: height 0.28s ease-in-out;
	}
	.collapse-controll {
		position: absolute;
		bottom: 0px;
		right: 8px;
		font-size: 14px;
		font-weight: 400;
		color: #6abcff;
		cursor: pointer;
	}
	.collapse-controll-icon {
		margin-right: 2px;
	}
</style>
