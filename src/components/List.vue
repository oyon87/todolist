<template>
	<ul class="list-group mt-4">
		<li class="list-group-item" v-for="(list, index) in lists" :key="index">
			<div class="row align-items-center">
				<div class="col">
					<span v-if="list.isDone"><del>{{ list.activity }}</del></span>
					<span v-else>{{ list.activity }}</span>
				</div>
				<div class="col-auto">
					<div class="row">
						<div class="col p-1">
							<button class="btn btn-info" @click="doneTODO(index)">Done</button>
						</div>
						<div class="col p-1">
							<button class="btn btn-danger" @click="deleteTODO(index)">X</button>
						</div>
					</div>
				</div>
			</div>
		</li>
	</ul>
</template>

<script>
export default {
	props: {
		lists: {
			type: Array,
			default: []
		}
	},
	setup(props, { emit }) {
		const deleteTODO = index => {
			emit('deleteTodo', index);
		};

		const doneTODO = index => {
			emit('doneTodo', index);
		};

		return {
			deleteTODO, doneTODO,
		}
	},
}
</script>