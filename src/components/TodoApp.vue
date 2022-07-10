<template>

	<div class="container">
		<h1 class="text-center mt-4">Vue Todo App</h1>

		<div class="d-flex">
			<input type="text"
			v-model="task"
			class="form-control" placeholder="Enter task">
			<button class="btn btn-success"
			@click="submitTask">Submit</button>
		</div>

		<table class="table table-bordered mt-5">
			<tr>
				<th class="w-50">Task</th>
				<th class="text-center">Status</th>
				<th class="text-center">#</th>
				<th class="text-center">#</th>
			</tr>
			<tr v-for="(task,index) in tasks" :key="index">
				<td :class="{finished: task.status == 'finished', 'in-progress': task.status=='in progress'}">{{task.name}}</td>
				<td class="text-center"
				@click="changeStatus(index)">{{task.status}}</td>
				<td class="text-center">
					<span class="fa fa-pencil"
					@click="editTask(index)"></span>
				</td>
				<td class="text-center">
					<span class="fa fa-trash"
					@click="deleteTask(index)"></span>
				</td>
			</tr>
		</table>
	</div>

</template>

<script>
export default {
	name: 'TodoApp',
	data() {
		return {
			task: '',
			editedTask: null,
			availableStatuses: ['to-do','in progress', 'finished'],
			tasks: []
		}
	},
	methods: {
		submitTask() {
			if (this.task.length === 0) return;
			if (this.editedTask === null) {
				this.tasks.push({
					name: this.task,
					status: 'to-do'
				})
			}
			else {
				this.tasks[this.editedTask].name = this.task;
				this.editedTask = null;
			}
			this.task = '';
		},
		deleteTask(index) {
			this.tasks.splice(index,1)
		},
		editTask(index) {
			this.task = this.tasks[index].name;
			this.editedTask = index;
		},
		changeStatus(index) {
			let newStatus = this.availableStatuses.indexOf(this.tasks[index].status);
			if (++newStatus > 2) newStatus = 0;
			this.tasks[index].status = this.availableStatuses[newStatus];
		}
	}
}
</script>

<style>
.in-progress {
	color: green;
}
.finished {
	text-decoration: line-through;
}
</style>