<template>
	<div v-for="task in tasks" :key="task.id">
		<Task :task="task" />
	</div>
</template>

<script>
import Task from './Task.vue';

export default {
	name: 'Tasks',
	props: ['tasks'],
	components: { Task }
};
</script>

<style></style>
