<script context="module">
	export const TABS = {};
</script>

<script>
	import { setContext, onDestroy } from 'svelte';
	import { writable } from 'svelte/store';

	const tabs = [];
	const panels = [];
	const selectedTab = writable(null);
	const selectedPanel = writable(null);

	setContext(TABS, {
		registerTab: tab => {
			tabs.push(tab);
			selectedTab.update(current => current || tab);
			
			onDestroy(() => {
				const i = tabs.indexOf(tab);
				tabs.splice(i, 1);
				selectedTab.update(current => current === tab ? (tabs[i] || tabs[tabs.length - 1]) : current);
			});
		},

		registerPanel: panel => {
			panels.push(panel);
			selectedPanel.update(current => current || panel);
			
			onDestroy(() => {
				const i = panels.indexOf(panel);
				panels.splice(i, 1);
				selectedPanel.update(current => current === panel ? (panels[i] || panels[panels.length - 1]) : current);
			});
		},

		selectTab: tab => {
			const i = tabs.indexOf(tab);
			selectedTab.set(tab);
			selectedPanel.set(panels[i]);
		},

		selectedTab,
		selectedPanel
	});
</script>

<style>
	.tabs {
		padding-top: 0em;
		grid-column: page;
	}

	figure {
		margin-bottom: 1.5em;
	}

	@media(max-width: 768px) {

        .tabs {
            grid-column: screen;
		}
		
	}
</style>

<figure class="subgrid">
	<div class="tabs interactive-container">
		<slot></slot>
	</div>
	<figcaption style="grid-column: text;"><a class="figure-number" href="#applications-tab">2</a>: Interactive articles are applicable to variety of domains, such as research dissemination, journalism, education, and policy and decision making.</figcaption>
</figure>
