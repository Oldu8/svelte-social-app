<script>
	import '../app.css';

	let { children } = $props();
</script>

<div class="flex-it h-full overflow-y-hidden p-10">
	{@render children()}
</div>
