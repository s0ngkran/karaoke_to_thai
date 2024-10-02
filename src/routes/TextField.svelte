<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	export let placeholder = 'Enter text';
	export let value: string = '';
	$: dispatch('update', value);

	// to use this component
	// <TextField placeholder="type here..." on:update={(d) => (value = d.detail)} />


</script>

<input
	bind:value
	{placeholder}
	class="w-full px-4 py-2 bg-gray-100 text-gray-700 rounded-lg border focus:outline-none"
/>
