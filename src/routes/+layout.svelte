<script lang="ts">
	import '../app.css';
	import { ModeWatcher } from 'mode-watcher';
	import { navigating } from '$app/stores';
	import Loading from '$lib/components/Loading.svelte';
	import Header from '$lib/components/Header.svelte';
	import Footer from '$lib/components/Footer.svelte';
	import { onMount } from 'svelte';

	let onMobile: boolean;

	onMount(() => {
		const mediaQuery = window.matchMedia('(max-width: 768px)');
		onMobile = mediaQuery.matches;
		mediaQuery.addEventListener('change', (mediaQuery) => {
			onMobile = mediaQuery.matches;
		});
	});
</script>
<ModeWatcher themeColors={{ dark: 'black', light: 'white' }} />
<div class="min-h-screen">
	<Header {onMobile} />
	<slot />
</div>
<Footer {onMobile} />
