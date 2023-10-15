<script>
	// @ts-nocheck

	import Header from '../../components/Header.svelte';
	import ReviewFooter from '../../components/ReviewFooter.svelte';
	import { onMount } from 'svelte';

	import {
		questionStore,
		chooseAnsStore,
		correctAnswerStore,
		answerChosenStore
	} from '../../store/store';

	let ansChosenByStudentArr = [];
	let correctAnsArr = [];

	$: for (let i = 0; i < $questionStore.length; i++) {
		let correctIndex = 0;
		if ($chooseAnsStore[i]) {
			for (let j = 0; j < 4; j++) {
				if (JSON.parse($questionStore[i].content_text).answers[j].answer == $chooseAnsStore[i]) {
					correctIndex = j;
				}
			}
		} else {
			correctIndex = null;
		}
		ansChosenByStudentArr[i] = correctIndex;
		$answerChosenStore[i] = correctIndex;
	}

	$: for (let i = 0; i < $questionStore.length; i++) {
		let actualCorrect = 0;
		for (let j = 0; j < 4; j++) {
			if (JSON.parse($questionStore[i].content_text).answers[j].is_correct == '1') {
				actualCorrect = j;
			}
		}
		correctAnsArr[i] = actualCorrect;
		$correctAnswerStore[i] = actualCorrect;
	}

	let questionNo;
	let allQuestions = [];
	onMount(async () => {
		const response = await fetch('data/question.json');
		allQuestions = await response.json();
		questionNo = new URL(location.href).searchParams.get('qno');
	});
	// afterUpdate(() => {
	//   console.log("url updated");
	// });
</script>

<div class="mycontainer">
	<Header />
	<div class="container containerQues">
		{#each allQuestions as data, i}
			{#if questionNo - 1 === i}
				<div class="">
					<span class="h4">
						{i + 1} . {JSON.parse(data.content_text).question}
					</span>
					<div class="d-flex flex-column mt-4 font-weight-bold">
						{#each JSON.parse(data.content_text).answers as answers, j}
							<div class="d-flex">
								{#if answers.is_correct == 1}
									<p class="h6 my-auto">{String.fromCharCode(65 + j)}</p>
									<label class="w-100 m-2 text-success">
										<span class="p-1 bg-success">
											<input type="radio" />
										</span>
										{@html answers.answer}
									</label>
								{:else if j === ansChosenByStudentArr[i] && correctAnsArr[i] !== ansChosenByStudentArr[i]}
									<!-- else if content here -->
									<p class="h6 my-auto">{String.fromCharCode(65 + j)}</p>
									<label class="w-100 m-2 text-danger">
										<span class="p-1 bg-danger">
											<input type="radio" class="bg-danger" checked />
										</span>
										{@html answers.answer}
									</label>
								{:else}
									<p class="h6 my-auto">{String.fromCharCode(65 + j)}</p>
									<label class="w-100 m-2">
										<input type="radio" disabled />
										{@html answers.answer}
									</label>
								{/if}
							</div>
						{/each}
						<div class="mt-3">
							{@html JSON.parse(allQuestions[questionNo - 1].content_text).explanation}
						</div>
					</div>
				</div>
			{/if}
		{/each}
	</div>
</div>

<div class="position-fixed w-50" style="bottom:15px;right:15px">
	<ReviewFooter
		{questionNo}
		on:prevPage_rev={() => {
			questionNo = parseInt(questionNo) - 1;
			console.log('questionNo : ', questionNo);
		}}
		on:nextPage_rev={() => {
			questionNo = parseInt(questionNo) + 1;
			console.log('questionNo : ', questionNo);
		}}
		on:updateQues_rev={(event) => {
			questionNo = event.detail;
		}}
	/>
</div>

<style>
	/* body {
		color: white;
	} */
	:global(seq::before) {
		content: attr(no);
		text-transform: uppercase;
	}

	.mycontainer {
		min-height: 100vh;
		min-width: 925px !important;
	}

	.containerQues {
		margin-top: 50px;
	}
</style>
