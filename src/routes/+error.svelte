<script lang="ts">
    import { onMount } from "svelte";
    let hostname = './';
    onMount(() => {
        hostname = window.location.origin;
    });
</script>
<div class="flex flex-col items-center justify-center font-bold pt-[30vh] px-4">
    <p class="text-2xl md:text-4xl font-bold pb-2">404</p>
    <h1 class="text-4xl md:text-6xl font-bold">Page not Found</h1>
    <a href={hostname} class="text-3xl font-bold pt-5">Go Home</a>
</div>