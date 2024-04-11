<script lang="ts">
    import type { StravaActivity } from '$lib/activities.js'
    import FrequencyHistogram from "./charts/FrequencyHistogram.svelte";
    
    export let activities: StravaActivity[];
</script>

<FrequencyHistogram {activities} />