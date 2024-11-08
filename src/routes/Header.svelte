<script lang="ts">
	import type { Snippet } from 'svelte';

	let {
		name,
		fake_name = 'Bartek',
		children,
		second_child
	}: { name: string; fake_name?: string; children: Snippet; second_child: Snippet } = $props();
</script>

<div>
	<h1>{name ? name : 'User'}'s Form!</h1>

	{@render children()}

	<h3>{@render second_child()}</h3>
</div>
<!-- <h2>{name.replaceAll('A', 'a')}</h2> -->
<!-- <p>{fake_name}</p> -->
