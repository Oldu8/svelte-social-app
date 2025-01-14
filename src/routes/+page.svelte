<script>
	let count = 0;
	// eto useEffect v svelte
	$: {
		if (count > 9) {
			console.log('обнуляй');
			count = 0;
		}
		console.log(count);
	}

	$: doubled = count * 2;
</script>

<div>
	<h2 class="text-3xl font-bold text-white underline">All count:</h2>
	<button
		on:click={() => count++}
		class="rounded border-2 border-white p-2 text-3xl font-bold text-white underline"
		>click: {count}</button
	>
	<p class="text-3xl text-white">Double: {doubled}</p>
</div>

<style>
</style>
