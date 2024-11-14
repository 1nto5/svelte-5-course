<script lang="ts">
	import { error } from '@sveltejs/kit';
	import { fly } from 'svelte/transition';
	import Header from './Header.svelte';

	let form_state = $state({
		answers: {},
		step: 0,
		error: ''
	});

	const QUESTIONS = [
		{
			question: 'What is your name?',
			id: 'name',
			type: 'text'
		},
		{
			question: 'What is your birthday?',
			id: 'bday',
			type: 'date'
		},
		{
			question: 'What is your favorite color?',
			id: 'color',
			type: 'color'
		}
	];

	function next_step(id: string) {
		if (!form_state.answers[id]) {
			form_state.error = 'Please fill out the form';
			return;
		}
		form_state.error = '';
		form_state.step++;
	}

	// will run on mount
	$effect(() => {
		// will run on mount
		console.log('on mount');
		// will run on unmount
		return () => console.log('on unmount');
	});

	$effect(() => {
		// will run when form_state.step changed
		console.log('form_state.step change', form_state.step);
		// DON'T create state based of other state, in effect
		// use $derived() instead
		// before effect Re-runs
		return () => console.log('before forma_state.step reruns', form_state.step);
	});
</script>

<Header name={form_state.answers.name}
	>Test children
	{#snippet second_child()}
		<p>Second child {'Test'}</p>
	{/snippet}
</Header>
<main>
	{#if form_state.step >= QUESTIONS.length}
		<p>Form complete!</p>
		<pre>{JSON.stringify(form_state.answers, null, 2)}</pre>
	{:else}
		<p>Step: {form_state.step + 1}</p>
	{/if}

	<!-- {@render formStep({
		id: 'name',
		question: 'What is your name?',
		type: 'type="text"'
	})} -->

	<!-- {#each QUESTIONS as question (question.id)} -->
	<!-- {#each QUESTIONS as { id, question, type } (question.id)}
		{@render formStep({
			id: id,
			question: question,
			type: type
		})}
	{/each} -->

	{#each QUESTIONS as question, index (question.id)}
		{#if form_state.step === index}
			<div
				in:fly={{ x: 200, duration: 200, opacity: 0, delay: 200 }}
				out:fly={{ x: -200, duration: 200, opacity: 0 }}
			>
				{@render formStep(question)}
			</div>
		{/if}
	{/each}

	{#if form_state.error}
		<p class="error">{form_state.error}</p>
	{/if}
</main>

{#snippet formStep({ id, question, type }: { id: string; question: string; type: string })}
	<article>
		<div>
			<label for={id}>{question}</label>
			<input {type} {id} bind:value={form_state.answers[id]} />
		</div>
	</article>
	<button onclick={() => next_step(id)}>Next Step</button>
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
