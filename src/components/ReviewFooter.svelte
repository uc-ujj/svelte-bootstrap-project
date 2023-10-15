<script>
	import { createEventDispatcher } from 'svelte';
	import SidePanel from './SidePanel.svelte';
	const dispatch = createEventDispatcher();
	export let questionNo;
	let sidebar_show = false;
	let inReviewFooter = true;

	const prevPage = () => {
		dispatch('prevPage_rev');
	};

	const nextPage = () => {
		dispatch('nextPage_rev');
	};

	const displayQuesNum = (event) => {
		dispatch('updateQues_rev', parseInt(event.detail) + 1);
	};
</script>

<div>
	<div class="d-flex justify-content-evenly p-2 align-items-center">
		<button on:click={() => (sidebar_show = !sidebar_show)} class="btn border"> List </button>
		<SidePanel bind:inReviewFooter bind:show={sidebar_show} on:displayQuesNum={displayQuesNum} />

		<a
			on:click={prevPage}
			href="/questionReview?qno={parseInt(questionNo)}"
			class="btn border"
			style={parseInt(questionNo) <= 1 ? 'pointer-events:none;' : ''}>Previous</a
		>
		<span class="">
			{parseInt(questionNo)} of 11
		</span>
		<a
			on:click={nextPage}
			href="/questionReview?qno={parseInt(questionNo)}"
			class="btn border"
			style={parseInt(questionNo) > 10 ? 'pointer-events:none;' : ''}>Next</a
		>
		<a class="" href="/">
			<button class="btn border">Dashboard</button>
		</a>
		<a class="" href="/result">
			<button class="btn border">Result</button>
		</a>
	</div>
</div>

<style>
</style>
