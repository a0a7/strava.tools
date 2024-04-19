<script lang="ts">
	import { slide } from 'svelte/transition';
	import { onMount } from 'svelte'; // @ts-expect-error: Exists
	import { signOut } from '@auth/sveltekit/client';
	import { page } from '$app/stores';

	import type { StravaActivity } from '$lib/activities.js'

	import * as Card from '$lib/components/ui/card';
	import * as Avatar from '$lib/components/ui/avatar';
	import { ScrollArea } from '$lib/components/ui/scroll-area/index.js';
	import Button from '$lib/components/ui/button/button.svelte';
	import { Separator } from '$lib/components/ui/separator';

	import Footer from '$lib/components/sidebar/Footer.svelte';
	import Header from '$lib/components/Header.svelte';
	import Charts from '$lib/components/Charts.svelte';
	import Loading from '$lib/components/Loading.svelte';
	import '$lib/components/sidebar/resizable-handle.css';
	import Landing from '$lib/components/Landing.svelte';
	import Profile from '$lib/components/Profile.svelte';
	import GlobalSettings from '$lib/components/GlobalSettings.svelte';
	import GlobalConfig from '$lib/components/GlobalConfig.svelte';
	import TypeSelector from '$lib/components/TypeSelector.svelte';

	let onMobile: boolean;

	let showConfig = false, showSettings = false, showTypeSelect = false;
	let activityTypeFilter: string;
	let commuteFilter: string;
	let dateRangeMinFilter: string;
	let rounding: number;
	let dateRangeMaxFilter: string;
	let showPrivate: boolean;
	let graphType: string;
	let scale: string, colorScheme: string;
	let activities: StravaActivity[] = [], error: string, isClient: boolean;

	async function getActivities() {
		try {
			if (
				// @ts-expect-error: Exists at runtime.
				$page.data?.session?.access_token &&
				new Date($page.data?.session?.expires) > new Date()
			) {
				const promises = Array.from({ length: 15 }, (_, i) =>
					fetch(
						// @ts-expect-error: Exists at runtime.
						`https://www.strava.com/api/v3/athlete/activities?access_token=${$page.data?.session?.access_token}&per_page=100&page=${i + 1}`
					).then((response) => {
						if (!response.ok) {
							throw new Error('HTTP Status ' + response.status);
						}
						return response.json();
					})
				);
				const allActivities = await Promise.all(promises);
				activities = allActivities.flat();
				
				let allCoordsArray: any = [];
				localStorage.setItem('activities', JSON.stringify(activities));
			} else {
				console.error('No access token found in session data. Reload page and try again.');
			}
		} catch (err) {
			error = String(err);
			console.error(err);
		}
	}
	onMount(() => {
		isClient = true;
		if (localStorage.getItem('activities')) {
			activities = JSON.parse(localStorage.getItem('activities')!);
		}
		const mediaQuery = window.matchMedia('(max-width: 768px)');
		onMobile = mediaQuery.matches;
		mediaQuery.addEventListener('change', (mediaQuery) => {
			onMobile = mediaQuery.matches;
		});
	});
	$: if (isClient && $page.data?.session) {
		if (activities.length == 0) {
			console.log('Requesting activities from Strava');
			getActivities();
		}
		console.log(activities);
	}
</script>


<svelte:head>
	<title>Strava Data Visualization Tools</title>
	<meta
		name="description"
		content="Datavis for Strava"
	/>
	<meta property="og:url" content="http://strava.tools/" />
	<meta property="og:image" content="" />
</svelte:head>
{#if $page.data.session?.user?.image && $page.data.session?.user?.name}
	<div class="flex flex-row items-center justify-center">
		<Card.Root class="mx-2 mt-3 w-[90vw] max-w-[1024px] ">
			<Card.Header class="flex flex-row items-center justify-center">
				<Profile />
			</Card.Header>
			<Card.Content class="max-w-full">
				<Separator class="mb-3 p-0"/>
				<div>
					<h2 class="font-bold text-2xl pb-1">
						{#if onMobile == true}
							<button class="cursor-pointer" on:click={() => showConfig = !showConfig}>
								<svg class={`text-foreground w-5 h-5 inline ${showConfig ? "rotate-180 -translate-y-1" : "rotate-90 -translate-y-0.5" }`} fill="currentColor" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><path d="M182.6 137.4c-12.5-12.5-32.8-12.5-45.3 0l-128 128c-9.2 9.2-11.9 22.9-6.9 34.9s16.6 19.8 29.6 19.8H288c12.9 0 24.6-7.8 29.6-19.8s2.2-25.7-6.9-34.9l-128-128z"/></svg>
								Filter Activities
							</button>
						{:else}
							Filter Activities
						{/if}
					</h2>
					{#if !onMobile || showConfig}
						<div class="w-full flex flex-row flex-wrap gap-3 md:pl-4" transition:slide>
							<GlobalSettings
								{activities}
								bind:activityTypeFilter
								bind:commuteFilter
								bind:dateRangeMinFilter
								bind:dateRangeMaxFilter
								bind:showPrivate
							/> 
						</div>
					{/if}
				</div>
				<Separator class="mt-3 mb-1 p-0"/>
				<div>
					<h2 class="font-bold text-2xl pb-1">
						{#if onMobile == true}
							<button class="cursor-pointer" on:click={() => showSettings = !showSettings}>
								<svg class={`text-foreground w-5 h-5 inline ${showSettings ? "rotate-180 -translate-y-1" : "rotate-90 -translate-y-0.5" }`} fill="currentColor" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><path d="M182.6 137.4c-12.5-12.5-32.8-12.5-45.3 0l-128 128c-9.2 9.2-11.9 22.9-6.9 34.9s16.6 19.8 29.6 19.8H288c12.9 0 24.6-7.8 29.6-19.8s2.2-25.7-6.9-34.9l-128-128z"/></svg>
								Global Settings
							</button>
						{:else}
							Global Settings
						{/if}
					</h2>
					{#if !onMobile || showSettings}
						<div class="w-full flex flex-row flex-wrap gap-3 md:pl-4" transition:slide>
							<GlobalConfig
								bind:rounding
								bind:colorScheme
							/>
						</div>
					{/if}
				</div>
				<Separator class="mt-3 mb-1 p-0"/>
				<div class="max-w-full">
					<h2 class="font-bold text-2xl pb-1">
						{#if onMobile == true}
							<button class="cursor-pointer" on:click={() => showTypeSelect = !showTypeSelect}>
								<svg class={`text-foreground w-5 h-5 inline ${showTypeSelect ? "rotate-180 -translate-y-1" : "rotate-90 -translate-y-0.5" }`} fill="currentColor" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><path d="M182.6 137.4c-12.5-12.5-32.8-12.5-45.3 0l-128 128c-9.2 9.2-11.9 22.9-6.9 34.9s16.6 19.8 29.6 19.8H288c12.9 0 24.6-7.8 29.6-19.8s2.2-25.7-6.9-34.9l-128-128z"/></svg>
								Show charts visualizing...
							</button>
						{:else}
							Show charts visualizing...
						{/if}
					</h2>
					{#if !onMobile || showTypeSelect}
						<div class="max-w-full" transition:slide>
							<TypeSelector value={graphType}/>
						</div>
					{/if}
				</div>
				<Charts {graphType} {activities} {activityTypeFilter} {commuteFilter} {dateRangeMinFilter} {dateRangeMaxFilter} {showPrivate} {scale} {colorScheme} {rounding}/>

			</Card.Content>
			<Card.Footer>
			</Card.Footer>
		</Card.Root>
	</div>
{:else}
	<Landing />
{/if}