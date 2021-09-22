<template>
	<div v-if="showAddTask">
		<AddTask @add-task="addTask" />
	</div>
	<Tasks
		@toggle-reminder="toggleReminder"
		@delete-task="deleteTask"
		:tasks="tasks"
	/>
</template>

<script>
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';

export default {
	name: 'App',
	props: ['showAddTask'],
	components: { Tasks, AddTask },
	data() {
		return {
			tasks: []
		};
	},
	methods: {
		async addTask(task) {
			const res = await fetch('http://localhost:3000/tasks', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify(task)
			});
			const data = await res.json();

			this.tasks = [...this.tasks, data];
		},
		async deleteTask(id) {
			if (confirm('Are you sure?')) {
				const res = await fetch(`http://localhost:3000/tasks/${id}`, {
					method: 'DELETE'
				});

				res.status === 200
					? (this.tasks = this.tasks.filter((task) => task.id !== id))
					: alert('Error deleting task');
			}
		},
		async toggleReminder(id) {
			const taskToToggle = await this.fetchTask(id);
			const updatedTask = {
				...taskToToggle,
				reminder: !taskToToggle.reminder
			};
			const res = await fetch(`http://localhost:3000/tasks/${id}`, {
				method: 'PUT',
				headers: {
					'Content-type': 'application/json'
				},
				body: JSON.stringify(updatedTask)
			});
			const data = await res.json();
			this.tasks = this.tasks.map((task) =>
				task.id === id ? { ...task, reminder: data.reminder } : task
			);
		},
		async fetchTasks() {
			const res = await fetch('http://localhost:3000/tasks');
			const data = await res.json();
			return data;
		},
		async fetchTask(id) {
			const res = await fetch(`http://localhost:3000/tasks/${id}`);
			const data = await res.json();
			return data;
		}
	},
	async created() {
		this.tasks = await this.fetchTasks();
	}
};
</script>

<style></style>
