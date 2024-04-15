<script lang="ts">
	import { onMount } from 'svelte';
	import * as Command from '$lib/components/ui/command/index.js';
	import * as Popover from '$lib/components/ui/popover/index.js'; // @ts-expect-error: this import does exist
	import Check from 'svelte-radix/Check.svelte'; // @ts-expect-error: this import does exist
	import CaretSort from 'svelte-radix/CaretSort.svelte';
	import { Label } from '$lib/components/ui/label/';
	import { Button } from '$lib/components/ui/button/index.js';
	import { Slider } from "$lib/components/ui/slider";
	import { cn } from '$lib/utils.js';
	import { tick } from 'svelte';
	import { mode } from 'mode-watcher';
	import QuestionMarkIcon from '$lib/components/QuestionMarkIcon.svelte';

	export let value: number[];

	onMount(() => {
		const savedValue = localStorage.getItem('radiusSliderValue');
		if (savedValue !== null) {
			value = [Number(savedValue)];
		} else {
			value = [0];
		}
	});

	$: if (typeof window !== 'undefined' && value) {
		localStorage.setItem('radiusSliderValue', value[0].toString());
	};
</script>

<div class="flex flex-col gap-0.5">
	<Label for="radius-slider" class="select-none text-sm font-medium"
		>Corner Rounding
		<div class="transform translate-y-[1px] inline-block">
			<QuestionMarkIcon content="Control the corner radius of graph elements" />
		</div></Label
	>
	<Slider id="radius-slider" bind:value max={100} step={1} class="w-[200px]i w pt-3 mx-2"/>
</div>