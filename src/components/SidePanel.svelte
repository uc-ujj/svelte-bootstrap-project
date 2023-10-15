<script>
	import { createEventDispatcher } from 'svelte';
	import { questionStore, chooseAnsStore } from '../store/store';

	const dispatch = createEventDispatcher();
	export let show = false;
	export let inReviewFooter;
	export let clicked_button;

	const displayQues = (i) => {
		dispatch('displayQuesNum', i);
	};

	clicked_button = 'all';
</script>

{#if show}
	<nav class="position-fixed start-0 d-flex flex-column p-2 pb-2" style="width:400px;top:58px;">
		<div class="d-flex mx-auto my-3 w-10">
			<button
				class="btn {`${clicked_button === 'all' ? 'bg-primary btn-primary' : 'bg-gray'}`}"
				on:click={() => (clicked_button = 'all')}>All</button
			>
			<button
				class="btn {`${clicked_button === 'attempted' ? 'bg-primary btn-primary' : 'bg-gray'}`}"
				on:click={() => (clicked_button = 'attempted')}>Attempted</button
			>
			<button
				class="btn {`${clicked_button === 'unattempted' ? 'bg-primary btn-primary' : 'bg-gray'}`}"
				on:click={() => (clicked_button = 'unattempted')}
			>
				Unattempted
			</button>
		</div>
		{#if clicked_button === 'attempted'}
			<!-- content here -->
			{#each $questionStore as data, i}
				{#if $chooseAnsStore[i]}
					<!-- content here -->
					<span
						class="p-1"
						style="cursor: pointer;"
						on:click={() => displayQues(i)}
						on:click={() => (show = false)}
					>
						<span class="btn btn-sm fw-bold">{i + 1}. </span>
						<span> {data.snippet}</span>
					</span>
					<hr />
				{/if}
			{/each}
		{:else if clicked_button === 'unattempted'}
			<!-- else if content here -->
			{#each $questionStore as data, i}
				{#if !$chooseAnsStore[i]}
					<!-- content here -->
					<span
						class="p-1"
						style="cursor: pointer;"
						on:click={() => displayQues(i)}
						on:click={() => (show = false)}
					>
						<span class="btn btn-sm fw-bold">{i + 1}. </span>
						<span> {data.snippet}</span>
					</span>
					<hr />
				{/if}
			{/each}
		{:else}
			<!-- else content here -->
			{#each $questionStore as data, i}
				<span
					class="p-1"
					style="cursor: pointer;"
					on:click={() => displayQues(i)}
					on:click={() => (show = false)}
				>
					<span class="btn btn-sm fw-bold">{i + 1}. </span>
					<span> {data.snippet}</span>
				</span>
				<hr />
			{/each}
		{/if}
	</nav>
{/if}

<style>
	nav {
		position: fixed;
		top: 0;
		left: 0;
		height: 100%;
		padding: 2rem 1rem 0.6rem;
		border-left: 1px solid #aaa;
		background: rgb(246, 244, 244);
		overflow-y: auto;
		width: 17rem;
		border-right: 1px solid black;
	}
</style>
