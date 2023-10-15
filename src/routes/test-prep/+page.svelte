<script>
	import Header from '../../components/Header.svelte';
	import { questionStore, chooseAnsStore } from '../../store/store.js';
	import { onMount } from 'svelte';
	import TestFooter from '../../components/TestFooter.svelte';

	let allQuestions = [];
	let chosenAnswer = [];

	let currentQuestion = 0;

	onMount(async () => {
		$chooseAnsStore = chosenAnswer;
		const response = await fetch('data/question.json');
		allQuestions = await response.json();
		questionStore.set(allQuestions);
	});
</script>

<div class="">
	<Header />
	<div class="container containerQues">
		{#each allQuestions as data, i}
			{#if currentQuestion === i}
				<div>
					<span class="h4">
						{i + 1} . {JSON.parse(data.content_text).question}
					</span>
					<div class="d-flex flex-column mt-4 font-weight-bold">
						{#each JSON.parse(data.content_text).answers as answers, j}
							<div class="d-flex align-items-center p-2">
								<label class="w-100 pointer font-weight-bold" style="cursor: pointer;">
									<input
										type="radio"
										class="m-2 form-check-input"
										value={answers.answer}
										name="radio"
										id="radio{j}"
										bind:group={chosenAnswer[i]}
									/>

									{@html answers.answer}
								</label>
							</div>
						{/each}
					</div>
				</div>
			{/if}
		{/each}
	</div>
</div>

<div class="position-fixed w-50" style="bottom:15px;right:15px">
	<TestFooter
		{currentQuestion}
		on:prevPage={() => (currentQuestion = currentQuestion - 1)}
		on:nextPage={() => (currentQuestion = currentQuestion + 1)}
		on:updateQues={(event) => {
			currentQuestion = event.detail;
		}}
	/>
</div>

<style>
	.containerQues {
		margin-top: 50px;
	}
</style>
