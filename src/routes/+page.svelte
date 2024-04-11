<script lang="ts">
	import Loading from '$lib/components/Loading.svelte';
	import type { PaneAPI } from 'paneforge';
	import { ScrollArea } from '$lib/components/ui/scroll-area/index.js';
	import '$lib/components/sidebar/resizable-handle.css';
	import Footer from '$lib/components/sidebar/Footer.svelte';
	import { onMount } from 'svelte'; // @ts-expect-error: Exists
	import { signOut } from '@auth/sveltekit/client';
	import Header from '$lib/components/Header.svelte';
	import * as Card from '$lib/components/ui/card';
	import * as Avatar from '$lib/components/ui/avatar';
	import Button from '$lib/components/ui/button/button.svelte';
	import { Separator } from '$lib/components/ui/separator';
	import Charts from '$lib/components/Charts.svelte';
	import type { StravaActivity } from '$lib/activities.js'

	import { page } from '$app/stores';
	console.log($page.data.session);
	export let onMobile: boolean;
	import Landing from '$lib/components/Landing.svelte';
	import Profile from '$lib/components/Profile.svelte';
	import GlobalSettings from '$lib/components/GlobalSettings.svelte';
	let activityTypeFilter: string;
	let commuteFilter: string;
	let dateRangeMinFilter: string;
	let dateRangeMaxFilter: string;
	let showPrivate: boolean;

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
		<Card.Root class="mx-2 mt-3 md:w-[90vw] max-w-[1024px]">
			<Card.Header class="flex flex-row items-center justify-center">
				<Profile />
			</Card.Header>
			<Card.Content>
				<Separator class="mb-3 p-0"/>
				<GlobalSettings
					{activities}
					bind:activityTypeFilter
					bind:commuteFilter
					bind:dateRangeMinFilter
					bind:dateRangeMaxFilter
					bind:showPrivate
				/>
				<Separator class="mt-3 mb-1 p-0"/>
				<Charts {activities} />

			</Card.Content>
			<Card.Footer>
			</Card.Footer>
		</Card.Root>
	</div>
{:else}
	<Landing />
{/if}