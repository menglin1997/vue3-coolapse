<template>
	<Collapse :default-height="90" v-model="isOpen" hide-controll>
		<div>这是内容...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
		<div>样式需要自己添加...</div>
	</Collapse>
	<span class="btn" @click="isOpen = !isOpen">{{ isOpen ? '关' : '开' }}</span>
</template>

<script setup>
	import Collapse from './components/Collapse.vue'
  import { ref } from 'vue';
  const isOpen = ref(false)
</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
</style>
