<script lang="ts">
	import Loading from '$lib/components/Loading.svelte';
	import type { PaneAPI } from 'paneforge';
	import { ScrollArea } from '$lib/components/ui/scroll-area/index.js';
	import '$lib/components/sidebar/resizable-handle.css';
	import Footer from '$lib/components/sidebar/Footer.svelte';
	import { onMount } from 'svelte';
	import Header from '$lib/components/Header.svelte';
	import { page } from '$app/stores';
	console.log($page.data.session);
	export let onMobile: boolean;
</script>


<svelte:head>
	<title>Strava Data Visualization Tools</title>
	<meta
		name="description"
		content="Datavis for Strava"
	/>
	<meta name="keywords" content="strava.tools, Strava, tools" />
	<meta name="author" content="Alexander Weimer" />
	<meta name="subject" content="Sports" />
	<meta name="copyright" content="Alexander Weimer" />
	<meta name="topic" content="Sports" />
	<meta name="summary" content="Data visualization for Strava" />
	<meta
		property="og:description"
		content="Datavis for Strava"
	/>
	<meta property="og:site_name" content="strava.tools" />
	<meta property="og:type" content="website" />
	<meta property="og:url" content="http://strava.tools/" />
	<meta property="og:image" content="" />
	<meta property="fb:admins" content="268094773018996" />

	<meta name="apple-mobile-web-app-title" content="strava.tools" />
	<meta name="apple-mobile-web-app-capable" content="yes" />
	<meta name="apple-touch-fullscreen" content="yes" />
	<meta name="apple-mobile-web-app-status-bar-style" content="black" />
	<meta name="format-detection" content="telephone=no" />

	<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" type="image/png" />
	<link rel="manifest" href="/manifest.json" />
	<link rel="mask-icon" href="/safari-pinned-tab.svg" color="#5bbad5" />
	<meta name="msapplication-TileColor" content="#5bbad5" />
	<meta name="theme-color" content="#5bbad5" />

	<meta name="google-site-verification" content="5RpSoSWAxQUXZKIk4xonkx3A_XsZQqN_U0tRTTI0Fe0" />
</svelte:head>
<h1 style="position: absolute; visibility: hidden;">
	Strava.tools data visualization for biking and running.
</h1>
