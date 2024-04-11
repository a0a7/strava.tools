<script lang="ts">
	import { Separator } from '$lib/components/ui/separator';
	import { Label } from '$lib/components/ui/label';
	import { Switch } from '$lib/components/ui/switch';
	import CommuteSelect from '$lib/components/settings/CommuteSelect.svelte';
	import ActivityTypeSelect from '$lib/components/settings/ActivityTypeSelect.svelte';
	import DateRangePicker from '$lib/components/settings/DateRangePicker.svelte';
	import type { StravaActivity } from '$lib/activities';

	export let activities: StravaActivity[];
	export let activityTypeFilter: string;
	export let commuteFilter: string;
	export let dateRangeMinFilter: string;
	export let dateRangeMaxFilter: string;
	export let showPrivate: boolean = true;                    
</script>

<div class="w-full flex md:flex-row gap-3">
	<ActivityTypeSelect bind:value={activityTypeFilter} />
	<CommuteSelect bind:value={commuteFilter} />

	<DateRangePicker
		{activities}
		bind:startDate={dateRangeMinFilter}
		bind:endDate={dateRangeMaxFilter}
	/>
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
	<Separator class="w-auto" />
</div>
