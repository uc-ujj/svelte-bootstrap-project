<script>
	//this is the prop that you will pass where the component is declared
	export let show = false;
</script>

<!-- Display the modal if show is true -->
{#if show}
	<div
		class="position-fixed top-50 start-50 d-flex justify-content-center align-items-center w-100 h-100"
		style="transform: translate(-50%, -50%);background-color: rgba(0, 0, 0, 0.3);"
	>
		<div class="rounded text-center border bg-white" style="max-width: 400px;">
			<div class="p-3 fw-semibold d-flex justify-content-between">
				<span class="">Confirmation</span>
				<button class="btn-close" on:click={() => (show = false)} />
			</div>
			<div class="px-5 pb-4">
				<h5 class="text-dark mb-4">Do you want to end the test?</h5>
				<div class="buttons">
					<button
						style="margin-right: 15px;"
						class="btn border border-2"
						on:click={() => (show = false)}>NO</button
					>
					<a href="/result">
						<button class="btn btn-primary">YES</button>
					</a>
				</div>
			</div>
		</div>
	</div>
{/if}
