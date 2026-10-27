<script lang="ts">
	let activeLesson = 0;
	let clicks = 0;
	let isLive = true;

	const lessons = [
		{
			index: '01',
			title: 'Astro anatomy',
			tag: 'Foundations',
			description: 'Learn how Astro ships almost no JavaScript by default, then add it exactly where interaction needs to live.',
			code: `<main>\n  <h1>Fast by default</h1>\n  <Counter client:load />\n</main>`
		},
		{
			index: '02',
			title: 'Svelte islands',
			tag: 'Interaction',
			description: 'Drop a Svelte component into an Astro page and choose when it wakes up in the browser.',
			code: `---\nimport Counter from './Counter.svelte';\n---\n<Counter client:visible />`
		},
		{
			index: '03',
			title: 'Content & routes',
			tag: 'Structure',
			description: 'Use file-based routing for pages and content collections when your site starts to grow a library.',
			code: `src/pages/about.astro\nsrc/pages/notes/[slug].astro\nsrc/content.config.ts`
		},
		{
			index: '04',
			title: 'Ship the site',
			tag: 'Deploy',
			description: 'Build a static output and publish it to GitHub Pages with an adapter-free, dependable workflow.',
			code: `npm run build\n\n# output: dist/`
		}
	];

	$: lesson = lessons[activeLesson];

	function selectLesson(index: number) {
		activeLesson = index;
		clicks = 0;
	}
</script>

<div class="lab-shell">
	<aside class="lesson-nav" aria-label="Walkthrough lessons">
		<div class="nav-label">The route</div>
		<ol>
			{#each lessons as item, index}
				<li>
					<button class:active={activeLesson === index} onclick={() => selectLesson(index)}>
						<span class="lesson-index">{item.index}</span>
						<span>
							<strong>{item.title}</strong>
							<small>{item.tag}</small>
						</span>
					</button>
				</li>
			{/each}
			</ol>
			<div class="nav-footnote">
				<span class="pulse"></span>
				<span>Local playground<br /><b>Ready to explore</b></span>
			</div>
		</aside>

	<section class="lesson-panel" aria-live="polite">
		<div class="lesson-heading">
			<div>
				<span class="eyebrow">Lesson {lesson.index} / 04 · {lesson.tag}</span>
				<h2>{lesson.title}</h2>
			</div>
			<div class="step-count">0{activeLesson + 1}<span>/04</span></div>
		</div>
		<p class="lesson-description">{lesson.description}</p>

		<div class="workspace">
			<div class="code-window">
				<div class="window-bar"><span class="dot coral"></span><span class="dot gold"></span><span class="dot mint"></span><span class="file-name">{activeLesson === 1 ? 'src/components/Counter.svelte' : 'src/pages/index.astro'}</span></div>
				<pre><code>{lesson.code}</code></pre>
			</div>

			<div class="demo-window">
				<div class="demo-label"><span class="live-dot"></span> Interactive output</div>
				{#if activeLesson === 0 || activeLesson === 1}
					<div class="counter-demo">
						<div class="demo-kicker">A tiny Svelte island</div>
						<div class="counter-value">{clicks}<span> clicks</span></div>
						<button class="try-button" onclick={() => clicks++}>Increment <span>+</span></button>
						<p>State lives in the component, not the page.</p>
					</div>
				{:else if activeLesson === 2}
					<div class="route-demo">
						<div class="route-line"><span class="route-method">GET</span><span>/notes/hello-world</span><span class="route-status">200</span></div>
						<div class="route-line muted"><span class="route-method">GET</span><span>/about</span><span class="route-status">200</span></div>
						<div class="route-line muted"><span class="route-method">GET</span><span>/contact</span><span class="route-status">200</span></div>
					</div>
				{:else}
					<div class="ship-demo">
						<div class="ship-icon">↗</div>
						<div><strong>dist/</strong><p>Static output is ready.</p></div>
					</div>
				{/if}
			</div>
		</div>

		<div class="panel-footer">
			<button class="status-toggle" onclick={() => isLive = !isLive} aria-pressed={isLive}><span class:offline={!isLive} class="toggle-dot"></span>{isLive ? 'Live preview on' : 'Preview paused'}</button>
			<div class="pager">
				<button aria-label="Previous lesson" disabled={activeLesson === 0} onclick={() => selectLesson(activeLesson - 1)}>←</button>
				<button aria-label="Next lesson" disabled={activeLesson === lessons.length - 1} onclick={() => selectLesson(activeLesson + 1)}>→</button>
			</div>
		</div>
	</section>
</div>

<style>
	.lab-shell { display: grid; grid-template-columns: 220px minmax(0, 1fr); min-height: 500px; border: 1px solid var(--line); background: var(--paper); box-shadow: 0 22px 60px rgba(28, 42, 52, .12); }
	.lesson-nav { background: var(--ink); color: #d9e1dc; padding: 30px 22px 22px; display: flex; flex-direction: column; }
	.nav-label, .eyebrow, .demo-label, .demo-kicker { color: var(--lime); font-size: 10px; font-weight: 700; letter-spacing: .14em; text-transform: uppercase; }
	.lesson-nav ol { list-style: none; margin: 22px 0; padding: 0; }
	.lesson-nav li { border-top: 1px solid rgba(255,255,255,.1); }
	.lesson-nav li:last-child { border-bottom: 1px solid rgba(255,255,255,.1); }
	.lesson-nav button { width: 100%; display: flex; gap: 12px; border: 0; padding: 15px 0; color: #9ca9a3; background: none; text-align: left; cursor: pointer; }
	.lesson-nav button.active, .lesson-nav button:hover { color: white; }
	.lesson-nav button.active .lesson-index { color: var(--lime); }
	.lesson-index { color: #6c7872; font: 12px var(--mono); padding-top: 2px; }
	.lesson-nav strong, .lesson-nav small { display: block; }
	.lesson-nav strong { font-size: 13px; font-weight: 600; }
	.lesson-nav small { margin-top: 4px; color: #68766e; font-size: 10px; }
	.nav-footnote { display: flex; gap: 9px; align-items: center; margin-top: auto; color: #7d8c84; font-size: 10px; line-height: 1.5; }
	.nav-footnote b { color: #c4cec8; font-weight: 500; }
	.pulse, .live-dot, .toggle-dot { display: inline-block; width: 7px; height: 7px; border-radius: 50%; background: var(--lime); box-shadow: 0 0 0 4px rgba(183, 214, 83, .12); }
	.lesson-panel { padding: 45px 48px 28px; min-width: 0; }
	.lesson-heading { display: flex; justify-content: space-between; align-items: start; gap: 20px; }
	.eyebrow { color: var(--coral); }
	h2 { margin: 10px 0 0; color: var(--ink); font: 400 clamp(28px, 4vw, 46px)/1.05 var(--display); letter-spacing: -.03em; }
	.step-count { color: var(--ink); font: 500 27px var(--mono); }
	.step-count span { color: #aab2aa; font-size: 12px; }
	.lesson-description { max-width: 610px; margin: 18px 0 32px; color: #647069; font-size: 15px; line-height: 1.7; }
	.workspace { display: grid; grid-template-columns: minmax(0, 1.1fr) minmax(210px, .9fr); min-height: 225px; }
	.code-window, .demo-window { min-width: 0; }
	.code-window { background: #1c2928; color: #d9e5dc; }
	.window-bar { height: 39px; display: flex; align-items: center; gap: 7px; padding: 0 15px; background: #15201f; }
	.dot { width: 7px; height: 7px; border-radius: 50%; }
	.coral { background: #e88775; }.gold { background: #e5c477; }.mint { background: #90c49b; }
	.file-name { margin-left: 8px; color: #81928a; font: 10px var(--mono); }
	pre { margin: 0; padding: 28px 24px; overflow: auto; color: #d3e5d5; font: 13px/1.8 var(--mono); }
	.demo-window { display: flex; flex-direction: column; background: #f2f4ed; border: 1px solid #dfe5da; border-left: 0; }
	.demo-label { display: flex; align-items: center; gap: 8px; padding: 15px 17px; color: #718078; border-bottom: 1px solid #dfe5da; }
	.live-dot { width: 5px; height: 5px; box-shadow: none; }
	.counter-demo, .route-demo, .ship-demo { margin: auto; padding: 20px; }
	.demo-kicker { color: #819078; font-size: 9px; }
	.counter-value { margin: 10px 0 17px; color: var(--ink); font: 400 42px var(--display); letter-spacing: -.04em; }
	.counter-value span { color: #819078; font: 11px var(--mono); letter-spacing: 0; }
	.try-button { border: 0; padding: 10px 13px; color: #21302c; background: var(--lime); font: 600 11px var(--mono); cursor: pointer; }
	.try-button span { margin-left: 18px; font-size: 16px; line-height: 0; }
	.counter-demo p, .ship-demo p { margin: 14px 0 0; color: #85918a; font-size: 11px; }
	.route-demo { width: 100%; box-sizing: border-box; }
	.route-line { display: grid; grid-template-columns: 36px 1fr 28px; gap: 10px; padding: 10px 0; border-bottom: 1px solid #dfe5da; color: var(--ink); font: 11px var(--mono); }
	.route-line.muted { color: #9ca79e; }.route-method { color: #d4775e; font-size: 9px; }.route-status { color: #729a70; font-size: 9px; }
	.ship-demo { display: flex; align-items: center; gap: 14px; }.ship-icon { display: grid; place-items: center; width: 42px; height: 42px; color: var(--ink); background: var(--lime); font-size: 22px; }.ship-demo strong { color: var(--ink); font: 13px var(--mono); }
	.panel-footer { display: flex; justify-content: space-between; align-items: center; padding-top: 22px; }
	.status-toggle { display: flex; align-items: center; gap: 9px; border: 0; color: #7b8780; background: none; font: 10px var(--mono); cursor: pointer; padding: 0; }.toggle-dot.offline { background: #b3bdb5; box-shadow: none; }
	.pager { display: flex; gap: 7px; }.pager button { width: 33px; height: 30px; border: 1px solid #d8dfd6; color: var(--ink); background: transparent; cursor: pointer; }.pager button:disabled { color: #bbc3bc; cursor: not-allowed; }
	@media (max-width: 700px) { .lab-shell { grid-template-columns: 1fr; }.lesson-nav { padding: 22px; }.lesson-nav ol { display: grid; grid-template-columns: 1fr 1fr; gap: 0 16px; margin-bottom: 15px; }.lesson-nav button { padding: 11px 0; }.nav-footnote { margin-top: 10px; }.lesson-panel { padding: 30px 22px 22px; }.workspace { grid-template-columns: 1fr; }.demo-window { min-height: 190px; border-left: 1px solid #dfe5da; border-top: 0; } }
</style>
