<script lang="ts">
	import { onMount } from 'svelte';

    type DropdownOption = {
		label: string;
		value: string;
		description?: string;
	};

	export let label = 'Choose an option';
	export let options: DropdownOption[] = [];
	export let value = '';
	export let placeholder = 'Select an option';
	export let name = '';

	let isOpen = false;
	let highlightedIndex = 0;
	let trigger: HTMLButtonElement;
	let listbox: HTMLUListElement;

	$: selectedOption = options.find((option) => option.value === value);
	$: listboxId = `${name || 'dropdown'}-listbox`;
	$: selectedId = selectedOption ? `${listboxId}-${selectedOption.value}` : undefined;

	onMount(() => {
		const closeOnOutsideClick = (event: MouseEvent) => {
			if (!(event.target instanceof Node) || !trigger?.parentElement?.contains(event.target)) {
				isOpen = false;
			}
		};

		document.addEventListener('click', closeOnOutsideClick);
		return () => document.removeEventListener('click', closeOnOutsideClick);
	});

	function openMenu() {
		isOpen = true;
		highlightedIndex = Math.max(0, options.findIndex((option) => option.value === value));
	}

	function choose(option: DropdownOption) {
		value = option.value;
		isOpen = false;
		trigger?.focus();
	}

	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Escape') {
			isOpen = false;
			return;
		}

		if (event.key === 'Enter' || event.key === ' ') {
			event.preventDefault();
			if (!isOpen) {
				openMenu();
			} else if (options[highlightedIndex]) {
				choose(options[highlightedIndex]);
			}
			return;
		}

		if (event.key === 'ArrowDown' || event.key === 'ArrowUp') {
			event.preventDefault();
			if (!isOpen) openMenu();
			else highlightedIndex = (highlightedIndex + (event.key === 'ArrowDown' ? 1 : -1) + options.length) % options.length;
		}
	}
</script>

<div class="dropdown" class:open={isOpen}>
	<span class="dropdown-label" id={`${listboxId}-label`}>{label}</span>
	<button
		class="trigger"
		type="button"
		bind:this={trigger}
		aria-haspopup="listbox"
		aria-expanded={isOpen}
		aria-controls={listboxId}
		aria-labelledby={`${listboxId}-label`}
		onclick={() => isOpen ? isOpen = false : openMenu()}
		onkeydown={handleKeydown}
	>
		<span class:selected={selectedOption}>{selectedOption?.label || placeholder}</span>
		<span class="chevron" aria-hidden="true">⌄</span>
	</button>

	{#if isOpen}
		<ul class="menu" bind:this={listbox} id={listboxId} role="listbox" aria-labelledby={`${listboxId}-label`} tabindex="-1">
			{#each options as option, index}
				<li>
					<button
						class:highlighted={highlightedIndex === index}
						class:chosen={value === option.value}
						type="button"
						role="option"
						aria-selected={value === option.value}
						onmouseenter={() => highlightedIndex = index}
						onclick={() => choose(option)}
					>
						<span>{option.label}</span>
						{#if option.description}<small>{option.description}</small>{/if}
						{#if value === option.value}<b aria-hidden="true">✓</b>{/if}
					</button>
				</li>
			{/each}
		</ul>
	{/if}
</div>

<style>
	.dropdown { position: relative; min-width: 0; }.dropdown-label { display: block; margin-bottom: 9px; color: #78857c; font: 10px var(--mono); letter-spacing: .09em; text-transform: uppercase; }.trigger { width: 100%; display: flex; align-items: center; justify-content: space-between; gap: 18px; min-height: 48px; border: 1px solid #cad4c9; padding: 0 14px; color: var(--ink); background: #fcfcf7; text-align: left; cursor: pointer; font: 13px 'Trebuchet MS', Verdana, sans-serif; }.trigger:hover, .open .trigger { border-color: var(--coral); }.trigger span:first-child { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; color: #929d95; }.trigger span.selected { color: var(--ink); }.chevron { color: var(--coral); font-size: 18px; line-height: 1; transition: transform .15s ease; }.open .chevron { transform: rotate(180deg); }.menu { position: absolute; z-index: 10; right: 0; left: 0; max-height: 235px; overflow-y: auto; margin: 5px 0 0; padding: 5px; border: 1px solid #cad4c9; background: #fcfcf7; box-shadow: 0 15px 30px rgba(32, 48, 44, .13); list-style: none; }.menu li { margin: 0; }.menu button { position: relative; width: 100%; display: block; border: 0; padding: 10px 29px 10px 10px; color: var(--ink); background: transparent; text-align: left; cursor: pointer; font: 12px 'Trebuchet MS', Verdana, sans-serif; }.menu button.highlighted { background: #edf1e5; }.menu button.chosen { color: #a45c4c; }.menu small { display: block; margin-top: 3px; color: #87928b; font: 10px var(--mono); }.menu b { position: absolute; top: 10px; right: 10px; color: var(--coral); font: 12px var(--mono); }
</style>
