<script lang="ts">
	import Header from './Header.svelte';
	let name = $state('Adrian');
	let status: 'OPEN' | 'CLOSED' = $state('OPEN');
	let full_name = $derived(name + ' ' + 'Antosiak');

	// function toggle() {
	// 	status = status === 'OPEN' ? 'CLOSED' : 'OPEN';
	// }
	function onclick() {
		status = status === 'OPEN' ? 'CLOSED' : 'OPEN';
	}
</script>

<Header {name} fake_name="Krzyś" />
<h2>{full_name}</h2>

<input type="text" bind:value={name} />

<p>The store is now {status}</p>
<!-- <button onclick={toggle}>Toggle status</button> -->
<!-- <button {onclick}>Toggle status</button> -->
<button onclick={() => (status = status === 'OPEN' ? 'CLOSED' : 'OPEN')}>Toggle status</button>
