<template>
	<div v-for="task in tasks" :key="task.id">
		{{ task.text }}
	</div>
</template>

<script>
export default {
	name: 'Tasks',
	props: ['tasks']
};
</script>

<style></style>
