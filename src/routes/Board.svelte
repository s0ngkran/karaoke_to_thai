<script lang="ts">
	export let chars;
	export let eng;
	export let isDim = false;

	// create dispatcher
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	// update value
	const updateValue = (c: string) => {
		dispatch('update', c);
	};
</script>

<div class="row cc">
	<div
		class={`grid sm:grid-cols-8 grid-cols-5 w-[40rem] ${
			isDim ? 'bg-slate-600' : ''
		}  p-2 m-2 ronuded-lg shadow`}
	>
		{#each chars as c, ind}
			<div class="row cc">
				<button
					on:click={(e) => {
						updateValue(eng[ind]);
					}}
					class={`w-[3rem] row ie jc 
${isDim ? 'bg-slate-500' : 'bg-slate-600'}
 shadow rounded py-1 px-2 my-1 t2`}
				>
					<div class="text-white">{c}</div>
					<div class="ml-2 text-yellow-400 t">{eng[ind]}</div>
				</button>
			</div>
		{/each}
	</div>
</div>
