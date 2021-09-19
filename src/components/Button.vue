<template>
	<button @click="onClink()" :style="{ background: color }" class="btn">
		{{ text }}
	</button>
</template>

<script>
export default {
	name: 'Button',
	props: ['text', 'color'],
	methods: {
		onClink() {
			console.log('Clicked');
		}
	}
};
</script>

<style></style>
