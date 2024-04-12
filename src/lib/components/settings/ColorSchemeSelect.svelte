<script lang="ts">
	import * as Command from '$lib/components/ui/command/index.js';
	import * as Popover from '$lib/components/ui/popover/index.js'; // @ts-expect-error: this import does exist
	import Check from 'svelte-radix/Check.svelte'; // @ts-expect-error: this import does exist
	import CaretSort from 'svelte-radix/CaretSort.svelte';
	import { Label } from '$lib/components/ui/label/';
	import { Button } from '$lib/components/ui/button/index.js';
	import { cn } from '$lib/utils.js';
	import { onMount, tick } from 'svelte';
	import { mode } from 'mode-watcher';
	import QuestionMarkIcon from '$lib/components/QuestionMarkIcon.svelte';

	const colorSchemes = [
		{
			value: 'sqrt',
			label: 'Square Root'
		},
		{
			value: 'log',
			label: 'Logarithmic'
		},
		{
			value: 'linear',
			label: 'Linear'
		}
	];

	let open = false;
	export let value = colorSchemes[0].value;

	$: selectedValue = colorSchemes.find((f) => f.value === value)?.label;

	function closeAndFocusTrigger(triggerId: string) {
		open = false;
		tick().then(() => {
			document.getElementById(triggerId)?.focus();
		});
	}
	onMount(() => {
		const selectedValue = localStorage.getItem('colorScheme');

		if (selectedValue !== null) {
			value = selectedValue;
		}
	});

	$: if (typeof window !== 'undefined' && value) {
		localStorage.setItem('colorScheme', value);
	};
</script>

<div class="flex flex-col gap-0.5">
	<Label for="color-scheme-select" class="select-none text-sm font-medium"
		>Color Scheme
		<div class="transform translate-y-[1px] inline-block">
			<QuestionMarkIcon content="Customize the color scheme used for continuous values" />
		</div></Label
	>
	<Popover.Root bind:open let:ids>
		<Popover.Trigger asChild id="color-scheme-select" let:builder>
			<Button
				builders={[builder]}
				variant="outline"
				role="combobox"
				aria-expanded={open}
				class="w-[200px] justify-between"
			>
				{selectedValue}
				<CaretSort class="ml-2 h-4 w-4 shrink-0 opacity-50" />
			</Button>
		</Popover.Trigger>
		<Popover.Content class="w-[200px] p-0">
			<Command.Root>
				<Command.Group class="">
					{#each colorSchemes as scheme}
						<Command.Item
						class="w-full"
							value={scheme.value}
							onSelect={(currentValue) => {
								value = currentValue;
								closeAndFocusTrigger(ids.trigger);
							}}
						>
							{scheme.label}
						</Command.Item>
					{/each}
				</Command.Group>
			</Command.Root>
		</Popover.Content>
	</Popover.Root>
</div>
