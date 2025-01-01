<script>
	import { onMount } from 'svelte'

	const sets = [
		[0, 0],
		[0, 1],
		[0, 2],
		[0, 3],
		[0, 4],
		[0, 5],
		[0, 6],
		[1, 0],
		[1, 1],
		[1, 2],
		[1, 3],
		[1, 4],
		[1, 5],
		[1, 6],
		[2, 0],
		[2, 1],
		[2, 2],
		[2, 3],
		[2, 4],
		[2, 5],
		[2, 6],
		[3, 0],
		[3, 1],
		[3, 2],
		[3, 3],
		[3, 4],
		[3, 5],
		[3, 6],
		[4, 0],
		[4, 1],
		[4, 2],
		[4, 3],
		[4, 4],
		[4, 5],
		[4, 6],
		[5, 0],
		[5, 1],
		[5, 2],
		[5, 3],
		[5, 4],
		[5, 5],
		[5, 6],
		[6, 0],
		[6, 1],
		[6, 2],
		[6, 3],
		[6, 4],
		[6, 5],
		[6, 6]
	];
	const faces = [
		"",
		"✦",
		"✦ ✦",
		"✦✦✦",
		"✦ ✦\n✦ ✦",
		"✦ ✦\n✦\n✦ ✦",
		"✦✦✦\n✦✦✦"
	];

	let crate = $state([...sets.slice(1)]);
	let placers = $state([[], []]);
	let track = $state([...sets[0]]);

	function shuffle() {
		crate.sort(() => Math.random() - 0.5);
	}
	function restart() {
		crate = [...sets];
		shuffle();
		track = crate.pop();
		placers.forEach((placer, index) => {
			placers[index] = [
				crate.pop(),
				crate.pop(),
				crate.pop(),
				crate.pop(),
				crate.pop()
			];
		});
	}

	onMount(() => {
		restart();
	})

</script>


<div class="flex flex-wrap justify-center gap-5 py-5">
	<button class="underline underline-offset-4" onclick={() => { restart(); }}>Restart</button>
</div>

<div class="flex flex-wrap justify-center gap-3 pb-5 text-center">
	<div class="table-row border-2 border-dashed divide-x-2 divide-dashed">
		{#each track as face}
			<div class="table-cell align-middle w-12 h-12 whitespace-pre overflow-hidden leading-none">{faces[face]}</div>
		{/each}
	</div>
</div>

<div class="grid grid-cols-1 md:grid-cols-2 gap-5 text-center">
	{#each placers as placer, index}
		<div class="flex flex-wrap justify-center gap-3">
			<div class="">
				Player <span class="">{index + 1}</span><br />
				<button class=" underline underline-offset-4" onclick={() => { placers[index].push(crate.pop()); }}>
					Draw
				</button>
			</div>
			{#each placer as domino, idx}
				<div class="">
					<button class="table-row border-2 divide-x-2" onclick={() => {
						if (track[0] == domino[0] || track[1] == domino[1]) {
							track = placers[index].splice(idx, 1)[0];
						}
					}}>
						{#each domino as face}
							<div class="table-cell align-middle w-12 h-12 whitespace-pre overflow-hidden leading-none">{faces[face]}</div>
						{/each}
					</button>
				</div>
				{/each}
		</div>
	{/each}
</div>

 