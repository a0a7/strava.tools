<script lang="ts">
    import type { StravaActivity } from '$lib/activities.js'
	import { onMount } from 'svelte';
    import FrequencyHistogram from "./charts/FrequencyHistogram.svelte";
    
    export let activities: StravaActivity[];
	export let activityTypeFilter: string;
	export let commuteFilter: string;
	export let dateRangeMinFilter: string;
	export let dateRangeMaxFilter: string;
	export let showPrivate: boolean;
	export let scale: string;
    export let colorScheme: string;

    let filteredActivities: StravaActivity[] = [];
    $: filteredActivities = activities.filter((activity) => {
        if (activityTypeFilter && activityTypeFilter !== 'all' && !activityTypeFilter.split('-').includes(activity.type)) return false;
        if (commuteFilter && ((commuteFilter === 'onlyCommutes' && activity.commute === false) || (commuteFilter === 'excludeCommutes' && activity.commute === true))) return false;
        if (dateRangeMinFilter && new Date(activity.start_date_local) < new Date(dateRangeMinFilter)) return false;
        if (dateRangeMaxFilter && new Date(activity.start_date_local) > new Date(dateRangeMaxFilter)) return false;
        if (!showPrivate && activity.private) return false;
        return true;
    });
</script>

<FrequencyHistogram activities={filteredActivities} />