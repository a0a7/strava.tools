<script lang="ts">
	import '../app.css';
	import { ModeWatcher } from 'mode-watcher';
	import { navigating } from '$app/stores';
	import Loading from '$lib/components/Loading.svelte';
	import Header from '$lib/components/Header.svelte';
	import Footer from '$lib/components/Footer.svelte';
	import { onMount } from 'svelte';

	let onMobile: boolean;

	$: deviceTypeKnown = typeof onMobile !== 'undefined';

	onMount(() => {
		onMobile = window.innerWidth <= 768;
	});
</script>

<ModeWatcher themeColors={{ dark: 'black', light: 'white' }} />
<div class="min-h-screen">
	<Header {onMobile} />
	{#if $navigating}
		<Loading />
	{/if}
	<slot {onMobile}/>
</div>
<Footer {onMobile} />
