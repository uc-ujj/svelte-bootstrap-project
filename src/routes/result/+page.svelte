<script>
	import Header from '../../components/Header.svelte';
	import {
		questionStore,
		chooseAnsStore,
		correctAnswerStore,
		answerChosenStore
	} from '../../store/store';
	let incorrect = 0;
	let percentage = 0;
	let correct = 0;
	let ansChosenByStudentArr = [];
	let correctAnsArr = [];
	$: unselectedQues = 0;
	$: for (let i = 0; i < $questionStore.length; i++) {
		let correct_index = 0;
		if ($chooseAnsStore[i]) {
			for (let j = 0; j < 4; j++) {
				if (JSON.parse($questionStore[i].content_text).answers[j].answer == $chooseAnsStore[i]) {
					correct_index = j;
				}
			}
		} else {
			correct_index = null;
		}
		ansChosenByStudentArr[i] = correct_index;
		$answerChosenStore[i] = correct_index;
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
	$: for (let i = 0; i < ansChosenByStudentArr.length; i++) {
		if (ansChosenByStudentArr[i] != null) {
			if (ansChosenByStudentArr[i] == correctAnsArr[i]) {
				correct = correct + 1;
				percentage = Math.round((correct / 11) * 100);
			} else {
				incorrect = incorrect + 1;
			}
		} else {
			unselectedQues += 1;
		}
	}
</script>

<div class="">
	<Header />
	<div class="container d-flex justify-content-center mt-4 flex-column align-items-center">
		<div class="d-flex" style="width: 800px;">
			<div class="d-flex flex-column align-items-center btn border border-primary rounded w-50">
				<p class="dataStyle">{percentage} %</p>
				<h6 class=" textStyle">Result</h6>
			</div>
			<div
				class="d-flex flex-column align-items-center btn border border-primary rounded w-50 ms-3"
			>
				<p class="dataStyle">11</p>
				<h6 class=" textStyle">Total item</h6>
			</div>
			<div
				class="d-flex flex-column align-items-center btn border border-primary rounded w-50 ms-3"
			>
				<p class="dataStyle">{correct}</p>
				<h6 class=" textStyle">Correct</h6>
			</div>
			<div
				class="d-flex flex-column align-items-center btn border border-primary rounded w-50 ms-3"
			>
				<p class="dataStyle">{incorrect}</p>
				<h6 class=" textStyle">Incorrect</h6>
			</div>
			<div
				class="d-flex flex-column align-items-center btn border border-primary rounded w-50 ms-3"
			>
				<p class="dataStyle">{unselectedQues}</p>
				<h6 class=" textStyle">Unattempted</h6>
			</div>
		</div>
		<table class="table">
			<thead>
				<tr class="">
					<th scope="col">S.No.</th>
					<th scope="col">Questions</th>
					<th scope="col">Options</th>
					<th scope="col">Result</th>
				</tr>
			</thead>
			<tbody>
				{#each $questionStore as data, i}
					<tr>
						<th scope="row" class="">{i + 1}</th>
						<td>
							<a href="questionReview?qno={i + 1}" class="w-100 text-decoration-none {i}">
								{data.snippet}
							</a>
						</td>
						<td class="d-flex customStylesTD">
							{#each JSON.parse(data.content_text).answers as _, j}
								{#if correctAnsArr[i] === j}
									<!-- content here -->
									<p
										class="bg-success border d-flex justify-content-center align-items-center ms-2 rounded"
										style="width: 24px; height:24px "
									>
										{String.fromCharCode(65 + j)}
									</p>
								{:else if j === ansChosenByStudentArr[i] && ansChosenByStudentArr[i] !== correctAnsArr[i]}
									<!-- else if content here -->
									<p
										class="bg-danger border d-flex justify-content-center align-items-center ms-2 rounded"
										style="width: 24px; height:24px "
									>
										{String.fromCharCode(65 + j)}
									</p>
								{:else}
									<p
										class="border d-flex justify-content-center align-items-center ms-2 rounded"
										style="width: 24px; height:24px "
									>
										{String.fromCharCode(65 + j)}
									</p>
								{/if}
							{/each}
						</td>
						<td>
							{#if ansChosenByStudentArr[i] == correctAnsArr[i]}
								<span class="">Correct</span>
							{:else if ansChosenByStudentArr[i] == null}
								<span class="">Unattempted</span>
							{:else}
								<span class="">Incorrect</span>
							{/if}
						</td>
					</tr>
				{/each}
			</tbody>
		</table>
	</div>
	<a href="/">
		<div
			class="p-2 rounded text-white"
			style="background-color:#858585; position:fixed; bottom:45px; right:15rem"
		>
			Dashboard
		</div>
	</a>
</div>

<style>
	.mycontainer {
		min-width: 1120px !important;
	}
	.dataStyle {
		font-size: x-large;
		padding: 0px 0px;
	}
	.customStylesTD {
		height: 66px;
		align-items: center;
		justify-content: center;
	}
	.textStyle {
		font-size: 20px;
	}
</style>
