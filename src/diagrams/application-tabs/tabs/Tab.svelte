<script>
	import { getContext } from 'svelte';
	import { TABS } from './Tabs.svelte';

	const tab = {};
	const { registerTab, selectTab, selectedTab } = getContext(TABS);

	registerTab(tab);
</script>

<style>
	button {
		background: none;
		border: none;
		border-radius: 0;
		margin: 0;
		color: #bbb;
		font-size: 1em;
		padding: 1em;
		cursor: pointer;
		flex: 1;
		border-bottom: 3px solid var(--gray-bg);
		/* width: 25%; */
		/* border-left: 1px solid var(--gray-border); */
	}

	button:focus {
		outline:0;
	}
	
	.selected {
		border-bottom: 3px solid var(--orange);
		color: #333;
	}

	@media(max-width: 1000px) {

		button {
			padding: 1em 0.5em;
			font-size: 0.85em;
		}

	}
</style>

<button class:selected="{$selectedTab === tab}" on:click="{() => selectTab(tab)}">
	<slot></slot>
</button>