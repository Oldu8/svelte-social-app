<script>
	import TaskList from '../../components/task-manager/TaskList.svelte';
	import { taskListStore } from '../../stores/tasks';

	export let data;
</script>

<div class="flex-it h-full p-10">
	<div class="mb-6 text-2xl text-white">
		{data.appName} - {data.content}
	</div>
	<button
		on:click={taskListStore.addList}
		class="mb-3 flex cursor-pointer items-start text-xl font-bold text-white hover:underline"
	>
		+ Add List
	</button>
	<div class="flex-it h-full flex-1">
		<div class="flex-it h-full flex-row rounded-xl">
			{#each $taskListStore as list, listIdx (list.id)}
				<TaskList {list} {listIdx} />
			{/each}
		</div>
	</div>
</div>
