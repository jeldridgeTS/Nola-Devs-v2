<script lang="ts">
	import Carousel from '$lib/components/carousel.svelte';
	import EventCard from '$lib/components/event-card.svelte';
	import type { Event } from '$types';
	//
	import { Circle2 } from 'svelte-loading-spinners';

	import { onMount } from 'svelte';
	import toast, { Toaster } from 'svelte-french-toast';

	import type { PageData } from './$types';
	import { dev } from '$app/environment';

	export let data: PageData;
	let events: Event[] = data.events;

	const checkToday = async () => {
		data.events.forEach((e) => {
			if (
				e.start.date ===
				new Intl.DateTimeFormat('en-US', {
					month: 'short',
					day: 'numeric',
					year: 'numeric'
				}).format(new Date())
			) {
				toast(`${e.summary} is happening today!!`, { icon: '🎉' });
			}
		});
	};

	onMount(checkToday);
</script>

<div class="noladevs">
	<div class="card">
		<div class="groupCard">
			<p>
				Welcome to Nola Devs, a thriving and welcoming software developers group in the heart of New
				Orleans! Join our community of passionate developers united by values of collaboration,
				learning, and growth. Whether you're a seasoned pro or a coding beginner, find like-minded
				individuals to connect with. Explore opportunities to enhance your skills, build valuable
				connections, and make an impact in the dynamic world of software development. Come discover
				the buzz in our growing tech ecosystem!
			</p>
		</div>
		<div class="carousel">
			<Carousel />
		</div>
	</div>
	<div class="event-list">
		{#if !events.length}
			<div class="mx-auto my-auto">
				<Circle2 size="200" colorOuter="#5569E0" unit="px" durationOuter="1s" />
			</div>
		{:else}
			<!-- TODO: Add pull down animation here -->
			{#each events as e}
				<EventCard event="{e}" />
			{/each}
		{/if}
	</div>
</div>
<Toaster />

<style>
	.groupCard {
		position: absolute;
		height: 100%;
		width: 50%;
		padding: 5px;
		text-align: center;
		background-color: rgba(240, 248, 255, 0.487);
		backdrop-filter: blur(4px);
		-webkit-backdrop-filter: blur(4px);
	}
	.groupCard p {
		margin: auto;
	}
	.card {
		position: relative;
		overflow: auto;
		scroll-snap-type: y mandatory;
		display: grid;
		clip-path: fill-box;
		border-radius: 10px;
	}
	.noladevs {
		height: 100vh;
		padding: 5px;
		display: grid;
		grid-template-rows: [first] 40% [line2] auto;
		gap: 3px;
	}
	.event-list {
		scroll-behavior: smooth;
		overflow-y: auto;
		border-radius: 10px;
		clip-path: fill-box;
		display: flex;
		flex-direction: column;
		height: 100%;
		gap: 3px;
		scroll-snap-type: y mandatory;
	}
	.carousel {
		overflow: hidden;
	}
	@media only screen and (max-width: 800px) {
		.event-list {
			height: fit-content;
		}
		.groupCard {
			width: 100%;
		}
	}
</style>
