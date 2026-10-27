<script lang="ts">
	import { onMount } from 'svelte';

	type ToggleOption = {
		label: string;
		value: string[];
		description?: string;
	};

	export let label = 'Choose an option';
	export let options: ToggleOption[] = [];
	export let value: string[] = [];
	export let placeholder = 'Select an option';
	export let name = '';

	let highlightedIndex = 0;
	let trigger: HTMLButtonElement;

	$: selectedOptions = options.filter((option) => option.value.every((val) => value.includes(val)));
	$: selectedId = selectedOptions[0] ? `${name || 'toggle'}-${selectedOptions[0].value.join('-')}` : undefined;

	function toggle(option: ToggleOption) {
		const isSelected = option.value.every((val) => value.includes(val));
		if (isSelected) {
			value = value.filter((val) => !option.value.includes(val));
		} else {
			value = [...value, ...option.value];
		}
	}
	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Enter' || event.key === ' ') {
			event.preventDefault();
			if (options[highlightedIndex]) {
				toggle(options[highlightedIndex]);
			}
		}
	}
</script>

<div class="toggle-field">
	<label for={name}>{label}</label>
	<div class="toggle-wrap" role="group" aria-label={label} on:keydown={handleKeydown}>
		{#each options as option, index}
			<button
				id={`${name || 'toggle'}-${option.value.join('-')}`}
				class:selected={option.value.every((val) => value.includes(val))}
				on:click={() => toggle(option)}
				on:mouseenter={() => (highlightedIndex = index)}
				aria-pressed={option.value.every((val) => value.includes(val))}
			>
				<span class="toggle-label">{option.label}</span>
				{#if option.description}
					<small class="toggle-description">{option.description}</small>
				{/if}
			</button>
		{/each}
	</div>


</div>

<style>

</style>