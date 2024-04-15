<script lang="ts">
    import ScrollArea from "./ui/scroll-area/scroll-area.svelte";
    import Button from "./ui/button/button.svelte";
	import { onMount } from "svelte";
    
    const dataTypes = ["any", "distance", "elevation", "duration", "date", "speed", "calories", "heartRate" ];
    export let value: string = "";

    onMount(() => {
		const selectedValue = localStorage.getItem('graphTypeFilter');

		if (selectedValue !== null) {
			value = selectedValue;
		} else {
            value = dataTypes[0];
        }
	});

	$: if (typeof window !== 'undefined' && value) {
		localStorage.setItem('graphTypeFilter', value);
	};
</script>

<ScrollArea class="max-w-full pb-2 flex flex-row whitespace-nowrap" orientation="horizontal">
    {#each dataTypes as type}
        <Button
            class="mr-2 !py-[5px] px-3 h-fit hover:opacity-[95%] shadow-md transform active:scale-95 transition-transform {value === type ? 'bg-secondary hover:bg-secondary text-background' : 'bg-input hover:bg-input'}"
            value={type}
            on:click={() => {
                value = type;
            }}
        >
            {type.replace(/(^\w)|(\s\w)/g, m => m.toUpperCase()).replace(/([A-Z])/g, ' $1')}
        </Button>
    {/each}
</ScrollArea>

