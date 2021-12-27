<template>
	<div class="container" style="margin-top: 20px;">
		<div class="card">
			<div class="card-body">
				<h5 class="card-title text-center">SIMPLE TODO APP</h5>
				<div class="row">
					<div class="col-10">
						<input type="text" class="form-control" v-model="todo" @keyup.enter="add">
					</div>
					<div class="col-2">
						<button class="btn btn-success" @click="add">ADD</button>
					</div>
				</div>
				<List class="mb-3" :lists="lists" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
				<small>Total TODO : {{ totalTODO }}</small>
			</div>
		</div>	
	</div>
</template>

<script>
import { ref, toRefs, reactive, computed, onMounted } from "vue";
import List from "./components/List.vue";

export default {
	components: { List },
	setup() {
		const todo = ref('');
		const todos = reactive({
			lists: [],
		});

		onMounted(() => {
			const items = localStorage.getItem('todos');
			todos.lists = items ? JSON.parse(items) : [];
		});

		const totalTODO = computed(() => {
			return todos.lists.length;
		});

		const add = () => {
			if(todo.value != ''){
				todos.lists.unshift({
				activity: todo.value,
				isDone: false
			});
				todo.value = "";
				saveToLocalStorage();
			}	
		};

		const deleteTodo = todoIndex => {
			todos.lists = todos.lists.filter((list, index) => {
				if(index != todoIndex){
					return list;
				}
			});
			saveToLocalStorage();
		};

		const doneTodo = todoIndex => {
			todos.lists = todos.lists.filter((list, index) => {
				if(index == todoIndex){
					list.isDone ? list.isDone = false : list.isDone = true;
				}
				return list;
			});
			saveToLocalStorage();
		};

		const saveToLocalStorage = () => {
			localStorage.setItem('todos', JSON.stringify(todos.lists));
		};

		return{
			todo, ...toRefs(todos), add, deleteTodo, totalTODO, doneTodo, saveToLocalStorage,
		};
	},
}
</script>