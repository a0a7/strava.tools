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

	const scales = [
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
	export let value: string;

	$: selectedValue = scales.find((f) => f.value === value)?.label;

	function closeAndFocusTrigger(triggerId: string) {
		open = false;
		tick().then(() => {
			document.getElementById(triggerId)?.focus();
		});
	}
	onMount(() => {
		const selectedValue = localStorage.getItem('scaleValue');

		if (selectedValue !== null) {
			value = selectedValue;
		} else {
			value = scales[0].value;
		}
	});
</script>

<div class="flex flex-col gap-0.5">
	<Label for="scale-select" class="select-none text-sm font-medium"
		>Graph Scale
		<div class="transform translate-y-[1px] inline-block">
			<QuestionMarkIcon content="Scales the values by a specific function before they're graphed." />
		</div></Label
	>
	<Popover.Root bind:open let:ids>
		<Popover.Trigger asChild id="scale-select" let:builder>
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
					{#each scales as scale}
						<Command.Item
						class="w-full"
							value={scale.value}
							onSelect={(currentValue) => {
								value = currentValue;
								closeAndFocusTrigger(ids.trigger);
							}}
						>
							{scale.label}
						</Command.Item>
					{/each}
				</Command.Group>
			</Command.Root>
		</Popover.Content>
	</Popover.Root>
</div>
