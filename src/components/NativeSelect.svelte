<script lang="ts">
	export type SelectOption = {
		label: string;
		value: string;
	};

	export let label = 'Choose an option';
	export let options: SelectOption[] = [];
	export let value = '';
	export let placeholder = 'Select an option';
	export let name = '';
</script>

<div class="select-field">
	<label for={name}>{label}</label>
	<div class="select-wrap">
		<select id={name} {name} bind:value aria-label={label}>
			<option value="" disabled>{placeholder}</option>
			{#each options as option}
				<option value={option.value}>{option.label}</option>
			{/each}
		</select>
		<span class="chevron" aria-hidden="true">⌄</span>
	</div>
</div>

<style>
	.select-field { min-width: 0; }
	label { display: block; margin-bottom: 9px; color: #78857c; font: 10px var(--mono); letter-spacing: .09em; text-transform: uppercase; }
	.select-wrap { position: relative; }
	select { width: 100%; min-height: 48px; appearance: none; border: 1px solid #cad4c9; border-radius: 0; padding: 0 42px 0 14px; color: var(--ink); background: #fcfcf7; font: 13px 'Trebuchet MS', Verdana, sans-serif; cursor: pointer; }
	select:hover, select:focus-visible { border-color: var(--coral); outline: 2px solid rgba(215, 121, 96, .18); outline-offset: 2px; }
	.chevron { position: absolute; top: 50%; right: 14px; pointer-events: none; color: var(--coral); font-size: 18px; line-height: 1; transform: translateY(-50%); }
</style>
