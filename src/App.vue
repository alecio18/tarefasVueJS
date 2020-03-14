<template>
	<div id="app">
		<h1>Semana Atual</h1>
		<TaskProgress :progress="progress" />
		<NewTask @taskAdded="addTask" />
		<TaskGrid :tasks="tasks" 
		@taskDeleted="deleteTask"
		@taskStateChanged="toggleTaskState"
		/>

		<hr>

		<h1>Próxima Semana</h1>
		<TaskProgress :progress="progress2" />
		<NewTask @taskAdded="addTask2" />
		<TaskGrid :tasks="tasks2" 
		@taskDeleted="deleteTask2"
		@taskStateChanged="toggleTaskState2"


		/>
	</div>
</template>

<script>
import TaskProgress from './components/TaskProgress.vue'
import TaskGrid from './components/TaskGrid.vue'
import NewTask from './components/NewTask.vue'
export default {
	components: { TaskProgress, TaskGrid, NewTask },
	data(){
		return {
			tasks: [],
			tasks2: []				
		}
	},
	computed : {
		progress() {
			const total = this.tasks.length
			const done = this.tasks.filter(t => !t.pending).length
			return Math.round(done / total * 100) || 0
		},
		progress2() {
			const total = this.tasks2.length
			const done = this.tasks2.filter(t => !t.pending).length
			return Math.round(done / total * 100) || 0
		},

	},
	watch: {
		tasks: {
			deep: true,
			handler() {
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}
		},

		tasks2 : {
			deep: true,
			handler() {
				localStorage.setItem('tasks2', JSON.stringify(this.tasks2))
		}
			
		}

	},
	methods: {
		addTask(task) {
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0
			if(reallyNew) {
				this.tasks.push({
					name: task.name,
					pending: task.pending || true
				})
			}
		},
		deleteTask(id) {
			this.tasks.splice(id, 1)
		},
		toggleTaskState(i){
			this.tasks[i].pending = !this.tasks[i].pending
		},


		addTask2(task) {
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks2.filter(sameName).length == 0
			if(reallyNew) {
				this.tasks2.push({
					name: task.name,
					pending: task.pending || true
				})
			}
		},
		deleteTask2(id) {
			this.tasks2.splice(id, 1)
		},
		toggleTaskState2(i){
			this.tasks2[i].pending = !this.tasks2[i].pending
		}

	},
	created() {
		const jsonDados = localStorage.getItem('tasks')		
		const array = JSON.parse(jsonDados)
		this.tasks = Array.isArray(array) ? array : []

		const jsonDados2 = localStorage.getItem('tasks2')		
		const array2 = JSON.parse(jsonDados2)
		this.tasks2 = Array.isArray(array2) ? array2 : []
	}
}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
