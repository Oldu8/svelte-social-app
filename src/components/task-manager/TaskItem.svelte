<script>
	import Editable from './Editable.svelte';
	import { taskListStore } from '../../stores/tasks';

	export let task;
	export let listIdx;
	export let taskIdx;

	let value = task.text;

	function updateTask(event) {
		taskListStore.updateTask(
			{
				id: task.id,
				text: event.detail.value
			},
			listIdx
		);
	}

	function dragStart(e) {
		const data = { listIdx, taskIdx };
		e.dataTransfer.setData('text/plain', JSON.stringify(data));
	}
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<div
	draggable={true}
	on:dragstart={dragStart}
	class="flex-it mb-2 cursor-pointer rounded-xl border border-solid bg-slate-500 p-2"
>
	<div class="flex-it">
		<Editable bind:value on:editCancel={updateTask}>
			<div class="flex-it flex-row">
				<div class="flex flex-1">{task.text}</div>
				<div class="flex items-end hover:text-red-600">
					<!-- svelte-ignore a11y_consider_explicit_label -->
					<button
						on:click|stopPropagation={() => {
							taskListStore.removeTask(listIdx, taskIdx);
						}}
					>
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
					</button>
				</div>
			</div>
		</Editable>
	</div>
</div>
