<script lang="ts">
    import * as Card from "$lib/components/ui/card";
    import FullscreenChart from "$lib/components/charts/FullscreenChart.svelte";
    let chartContainer: HTMLDivElement;
    let chart: SVGElement | undefined;
    let card: HTMLDivElement;
    $: if (chartContainer) {
        chart = chartContainer.querySelector('svg') ?? undefined;
    }

    function fullscreenChart() {
        console.log(card, chart)
        if (card) {
            const el = card.cloneNode(true) as HTMLDivElement;
            const firstChild = el.firstElementChild as HTMLDivElement;
            const fullscreenButton = el.querySelector('.fullscreenButton') as HTMLButtonElement;
            const buttonContainer = el.querySelector('.buttonContainer') as HTMLDivElement;
            el.style.position = 'fixed';
            el.style.top = '0';
            el.style.left = '0';
            el.style.width = '100vw';
            el.style.height = '100vh';
            el.style.zIndex = '100';
            el.style.display = 'flex';
            el.style.justifyContent = 'center';
            el.style.alignItems = 'center';
            el.style.backdropFilter = 'blur(10px)';
            el.addEventListener('click', el.remove);
            fullscreenButton.innerHTML = '<svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512"><path d="M342.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L192 210.7 86.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L146.7 256 41.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L192 301.3 297.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L237.3 256 342.6 150.6z"/></svg>';
            firstChild.style.height = 'auto';
            firstChild.style.width = 'auto';
            firstChild.style.zIndex = '101';
            buttonContainer.style.paddingRight = '1px';
            buttonContainer.style.paddingTop = '1px'; 
            document.body.appendChild(el);
        }
    }
</script>
<div bind:this={card}>
    <Card.Root class="p-1 rounding">
        <Card.Header class="relative">
            <Card.Title><slot name="title" /></Card.Title>
            <Card.Description><slot name="description" /></Card.Description>
            <div class="buttonContainer absolute top-0 right-0 pr-5 pt-2 flex flex-row gap-3">
                <button >
                    <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M288 32c0-17.7-14.3-32-32-32s-32 14.3-32 32V274.7l-73.4-73.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3l128 128c12.5 12.5 32.8 12.5 45.3 0l128-128c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L288 274.7V32zM64 352c-35.3 0-64 28.7-64 64v32c0 35.3 28.7 64 64 64H448c35.3 0 64-28.7 64-64V416c0-35.3-28.7-64-64-64H346.5l-45.3 45.3c-25 25-65.5 25-90.5 0L165.5 352H64zm368 56a24 24 0 1 1 0 48 24 24 0 1 1 0-48z"/></svg>
                </button>
                <button class="fullscreenButton" on:click={fullscreenChart}>
                    <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><path d="M0 180V56c0-13.3 10.7-24 24-24h124c6.6 0 12 5.4 12 12v40c0 6.6-5.4 12-12 12H64v84c0 6.6-5.4 12-12 12H12c-6.6 0-12-5.4-12-12zM288 44v40c0 6.6 5.4 12 12 12h84v84c0 6.6 5.4 12 12 12h40c6.6 0 12-5.4 12-12V56c0-13.3-10.7-24-24-24H300c-6.6 0-12 5.4-12 12zm148 276h-40c-6.6 0-12 5.4-12 12v84h-84c-6.6 0-12 5.4-12 12v40c0 6.6 5.4 12 12 12h124c13.3 0 24-10.7 24-24V332c0-6.6-5.4-12-12-12zM160 468v-40c0-6.6-5.4-12-12-12H64v-84c0-6.6-5.4-12-12-12H12c-6.6 0-12 5.4-12 12v124c0 13.3 10.7 24 24 24h124c6.6 0 12-5.4 12-12z"/></svg>
                </button>
            </div>
        </Card.Header>
        <Card.Content>
            <slot name="options" />
            <div class="chartContainer" bind:this={chartContainer}>
                <slot name="chart" />
            </div>
        </Card.Content>
    </Card.Root>
</div>