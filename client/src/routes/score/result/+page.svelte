<script lang="ts">
	import redirectIfNoCaseSelected from '$lib/functions/redirectIfNoCaseSelected';
	import Smart from '$lib/methods/smart.svelte';
	import { activeCaseStudy } from '$lib/stores/caseStudyStore';
	import { onMount } from 'svelte';
	let calculationType: string;

	const methods = ['Smart'];
	$: console.log(calculationType);
	onMount(() => {
		redirectIfNoCaseSelected();
	});
</script>

<svelte:head>
	<title>SMART Result</title>
</svelte:head>

<div class="flex items-center gap-5">
	<h1 class="text-2xl font-bold sm:text-3xl">Matriks Perbandingan</h1>
</div>

<div class="flex flex-col mt-5">
	<div class="overflow-x-auto">
		<table class="table">
			<thead>
				<tr>
					<th>Alternatif</th>
					{#if $activeCaseStudy && $activeCaseStudy.criteria.length}
						{#each $activeCaseStudy.criteria as criteria}
							<th contenteditable="true">{criteria.title}</th>
						{/each}
					{/if}
				</tr>
			</thead>
			<tbody>
				{#if $activeCaseStudy && $activeCaseStudy.alternative.length}
					{#each $activeCaseStudy.alternative as alternative, i}
						<tr>
							<td>{alternative.title}</td>
							{#each alternative.score as score}
								<td>{score.score}</td>
							{/each}
						</tr>
					{/each}
				{/if}
			</tbody>
		</table>
	</div>
</div>

<div class="flex items-center gap-5 mt-5">
	<h1 class="text-2xl font-bold sm:text-3xl">Proses SMART</h1>
</div>

<div class="flex flex-col mt-5 gap-5">
	<div class="flex items-center gap-5">
		<label for="calculationType">Metode</label>
		<select
			name="calculationType"
			id="calculationType"
			bind:value={calculationType}
			class="select select-bordered select-sm"
		>
			<option value="">Pilih</option>
			{#each methods as method}
				<option value={method}>{method}</option>
			{/each}
		</select>
	</div>
</div>

{#if calculationType}
	{#if calculationType === 'Smart'}
		<Smart />
	{/if}
{/if}
