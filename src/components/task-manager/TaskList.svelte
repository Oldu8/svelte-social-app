<script context="module">
	import { writable } from 'svelte/store';

	let listHoverId = writable(null);
</script>

<script>
	import { taskListStore } from '../../stores/tasks';
	import TaskItem from './TaskItem.svelte';
	import { flip } from 'svelte/animate';
	import { fade, fly } from 'svelte/transition';
	import Editable from './Editable.svelte';

	export let list;
	export let listIdx;

	let value = list.text;

	function drop(e) {
		const sourceJson = e.dataTransfer.getData('text/plain');
		const sourceData = JSON.parse(sourceJson);
		taskListStore.moveTask(sourceData, listIdx);
		listHoverId.set(null);
	}

	function updateList(e) {
		taskListStore.updateList(e.detail.value, listIdx);
	}
</script>

<div class="flex-it m-2 my-0 h-full min-h-full w-80 max-w-sm">
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<div
		on:dragenter={() => {
			listHoverId.set(list.id);
		}}
		on:dragover|preventDefault={() => {}}
		on:drop={drop}
		class:hovering={list.id === $listHoverId}
		class="flex-it max-h-full rounded-xl border-2 border-gray-500 bg-slate-400"
	>
		<div class="flex-it m-3">
			<Editable bind:value on:editCancel={updateList}>
				<div class="flex-it flex-row">
					<div class="mr-2 text-left text-xl font-bold">{list.text}</div>
					<!-- svelte-ignore a11y_consider_explicit_label -->
					<button
						on:click|stopPropagation={() => {
							taskListStore.removeList(listIdx);
						}}
					>
						<div class="flex items-center hover:text-red-600">
							<svg
								xmlns="http://www.w3.org/2000/svg"
								width="16"
								height="16"
								fill="currentColor"
								class="bi bi-trash"
								viewBox="0 0 16 16"
							>
								<path
									d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"
								/>
								<path
									fill-rule="evenodd"
									d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"
								/>
							</svg>
						</div>
					</button>
				</div>
			</Editable>
		</div>
		<div class="overflow-y-auto overflow-x-hidden p-2">
			{#each list.items as task, taskIdx (task.id)}
				<div out:fade in:fly={{ y: 200 }} animate:flip>
					<TaskItem {task} {listIdx} {taskIdx} />
				</div>
			{/each}
		</div>
		<button on:click={() => taskListStore.addTask(listIdx)} class="flex p-2 underline">
			+ Add Task
		</button>
	</div>
</div>

<style>
	.hovering {
		border: 2px solid orange;
	}
</style>
