<script lang="ts">
	import NativeSelect from './NativeSelect.svelte';
    import Toggle from './Toggle.svelte';

	type DropdownOption = {
		label: string;
		value: string;
		description?: string;
	};

	const frameworks: DropdownOption[] = [
		{ label: 'Astro', value: 'astro' },
		{ label: 'Svelte', value: 'svelte' },
		{ label: 'TypeScript', value: 'typescript' },
		{ label: 'Vite', value: 'vite' }
	];
	const moods: DropdownOption[] = [
		{ label: 'Curious', value: 'curious', description: 'Ask one more question' },
		{ label: 'Focused', value: 'focused', description: 'Make the next small move' },
		{ label: 'Playful', value: 'playful', description: 'Try the weird idea' }
	];
	const densities: DropdownOption[] = [
		{ label: 'Comfortable', value: 'comfortable' },
		{ label: 'Compact', value: 'compact' },
		{ label: 'Spacious', value: 'spacious' }
	];

    const toggleOptions = [
        { label: 'Option A', value: ['a'], description: 'This is option A' },
        { label: 'Option B', value: ['b'], description: 'This is option B' },
        { label: 'Option C', value: ['c'], description: 'This is option C' }
    ];

	let framework = 'astro';
	let mood = '';
	let density = 'comfortable';
	let playgroundMessage = 'Pick an option to update this preview.';
    let toggleValue: string[] = [];


	$: playgroundMessage = framework === 'astro'
		? 'Astro keeps the page light until an island needs to wake up.'
		: framework === 'svelte'
			? 'Svelte keeps the component state close to the interaction.'
			: `You chose ${framework}. The component is still independent.`;
</script>

<div class="component-lab">
	<section class="preview-pane" aria-labelledby="preview-title">
		<div class="pane-topline"><span class="live-dot"></span> Live component preview <span class="preview-path">/components/dropdown</span></div>
		<div class="preview-content">
			<p class="preview-kicker">A reusable primitive</p>
			<h2 id="preview-title">Small controls,<br /><em>good manners.</em></h2>
			<p class="preview-copy">This select is a self-contained Svelte component around the native HTML control. Each instance keeps its own value while the browser handles keyboard, touch, and assistive technology behavior.</p>
			<div class="preview-message"><span class="message-mark">↳</span>{playgroundMessage}</div>
		</div>
	</section>

	<section class="controls-pane" aria-label="Dropdown component examples">
		<div class="pane-topline"><span class="code-mark">&lt;/&gt;</span> Try the instances</div>
		<div class="control-grid">
			<NativeSelect name="framework" label="Primary stack" options={frameworks} bind:value={framework} />
			<NativeSelect name="mood" label="Today's mode" options={moods} bind:value={mood} placeholder="Choose a mood" />
			<NativeSelect name="density" label="Layout density" options={densities} bind:value={density} />
		</div>
		<div class="selection-row"><span>Current values</span><code>{framework} · {mood || 'none'} · {density}</code></div>
	</section>

    <section>
        <div>
            <Toggle label="Example Toggle" options={toggleOptions} bind:value={toggleValue} />
		    <div class="selection-row"><span>Current values</span><code>{toggleValue}</code></div>
        </div>
    </section>
</div>

<style>
	.component-lab { display: grid; grid-template-columns: .9fr 1.1fr; border: 1px solid var(--line); background: var(--paper); box-shadow: 0 22px 60px rgba(28, 42, 52, .12); }.preview-pane { color: #dce6df; background: var(--ink); }.controls-pane { min-width: 0; background: #f2f4ed; }.pane-topline { display: flex; align-items: center; gap: 8px; min-height: 48px; box-sizing: border-box; padding: 0 22px; border-bottom: 1px solid rgba(255,255,255,.11); color: #92a098; font: 10px var(--mono); letter-spacing: .03em; }.controls-pane .pane-topline { border-color: #dfe5da; color: #7e8b82; }.live-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--lime); box-shadow: 0 0 0 4px rgba(183, 214, 83, .12); }.code-mark { color: var(--coral); font-weight: 700; }.preview-path { margin-left: auto; color: #596b63; font-size: 9px; }.preview-content { padding: 52px 42px 46px; }.preview-kicker { margin: 0; color: var(--lime); font: 10px var(--mono); letter-spacing: .14em; text-transform: uppercase; }.preview-content h2 { margin: 15px 0 0; color: white; font: 400 clamp(32px, 4vw, 53px)/.98 var(--display); letter-spacing: -.04em; }.preview-content h2 em { color: var(--coral); font-style: italic; }.preview-copy { max-width: 330px; margin: 25px 0 0; color: #9eada4; font-size: 13px; line-height: 1.65; }.preview-message { display: flex; align-items: start; gap: 10px; margin-top: 35px; padding-top: 15px; border-top: 1px solid rgba(255,255,255,.12); color: #d6e1d8; font: 11px/1.6 var(--mono); }.message-mark { color: var(--lime); font-size: 17px; }.control-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 25px 18px; padding: 39px 30px 31px; }.control-grid :global(.dropdown:first-child) { grid-column: 1 / -1; }.selection-row { display: flex; align-items: center; justify-content: space-between; gap: 15px; margin: 0 30px; padding: 15px 0 18px; border-top: 1px solid #d8dfd5; color: #8a958d; font: 10px var(--mono); }.selection-row code { overflow: hidden; color: var(--coral); font: 10px var(--mono); text-overflow: ellipsis; white-space: nowrap; }
	@media (max-width: 760px) { .component-lab { grid-template-columns: 1fr; }.preview-content { padding: 38px 25px; }.control-grid { padding: 30px 20px 22px; }.selection-row { margin: 0 20px; } }
	@media (max-width: 430px) { .control-grid { grid-template-columns: 1fr; }.control-grid :global(.dropdown:first-child) { grid-column: auto; } }
</style>
