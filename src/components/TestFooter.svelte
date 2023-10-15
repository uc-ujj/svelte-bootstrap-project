<script>
	// @ts-nocheck

	import { createEventDispatcher } from 'svelte';
	import Modal from './Modal.svelte';
	import SidePanel from './SidePanel.svelte';

	import Timer from './Timer.svelte';
	const dispatch = createEventDispatcher();
	export let currentQuestion;
	let sidebar_show = false;
	let confirm_show = false;

	const prevPage = () => {
		dispatch('prevPage');
	};

	const nextPage = () => {
		dispatch('nextPage');
	};

	const displayQuesNum = (event) => {
		dispatch('updateQues', event.detail);
	};
</script>

<div>
	<div class="d-flex justify-content-evenly p-2 align-items-center border">
		<span class="">
			<Timer />
		</span>

		<button on:click={() => (sidebar_show = !sidebar_show)} class="btnborder"> List </button>
		<SidePanel bind:show={sidebar_show} on:displayQuesNum={displayQuesNum} />

		<button
			on:click={prevPage}
			on:click={() => (sidebar_show = false)}
			class="btnborder"
			disabled={currentQuestion < 1 ? true : false}>Previous</button
		>
		<div class="btnborder">
			{currentQuestion + 1} of 11
		</div>
		<button
			on:click={nextPage}
			on:click={() => (sidebar_show = false)}
			class="btnborder"
			disabled={currentQuestion + 1 > 10 ? true : false}>Next</button
		>

		<button class="btnborder" on:click={() => (confirm_show = !confirm_show)}>End Test</button>
	</div>
</div>

<Modal bind:show={confirm_show} />

<style>
</style>
