<script lang="ts">
	import { Label } from '$lib/components/ui/label';
	import { Switch } from '$lib/components/ui/switch';
	import ColorSchemeSelect from './settings/ColorSchemeSelect.svelte';
	import ScaleSelect from './settings/ScaleSelect.svelte';
	import RadiusSlider from './settings/RadiusSlider.svelte';
	export let colorScheme: string;
	export let showPrivate: boolean = true; 
	export let rounding: number = 0;                   
</script>

<div class="flex flex-row gap-3">
	<ColorSchemeSelect bind:value={colorScheme} />
	<RadiusSlider bind:value={rounding}/>
</div>
<div class="flex flex-row gap-3">
	<div class="pl-2 pt-1 flex flex-col gap-0.5">
		<Label for="private-activities" class="bottom-1 pr-1 relative select-none text-sm font-medium"
			>Private Activities</Label
		>
		<Switch
			id="private-activities"
			bind:checked={showPrivate}
			class="data-[state=checked]:bg-secondary"
		/>
	</div>
</div>