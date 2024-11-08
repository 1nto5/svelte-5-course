<script lang="ts">
	import { error } from '@sveltejs/kit';
	import Header from './Header.svelte';

	let form_state = $state({
		name: '',
		birthday: 'OPEN',
		step: 0,
		error: ''
	});
</script>

<Header name={form_state.name}
	>Test children
	{#snippet second_child()}
		<p>Second child</p>
	{/snippet}
</Header>
<main>
	<p>Step: {form_state.step + 1}</p>

	{@render formStep({
		id: 'name',
		question: 'What is your name?',
		type: 'type="text"'
	})}

	{#if form_state.error}
		<p class="error">{form_state.error}</p>
	{/if}
</main>

{#snippet formStep({ id, question, type }: { id: string; question: string; type: string })}
	<article>
		<div>
			<label for={id}>{question}</label>
			<input {type} {id} bind:value={form_state[id]} />
		</div>
	</article>
{/snippet}

<style>
	/* works globally */
	/* :global(div) {
		background: blue;
	} */
	/* works only in this component */
	.error {
		color: red;
	}
</style>
