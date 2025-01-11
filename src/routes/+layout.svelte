<script>
	import '../app.css';
	let { children } = $props();
</script>

<div>
	<div>header</div>
	{@render children()}
	<div>footer</div>
</div>
