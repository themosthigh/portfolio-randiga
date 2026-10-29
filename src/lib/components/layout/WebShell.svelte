<script>
	import { page } from '$app/state';
	import Footer from './Footer.svelte';
	import PageContent from './PageContent.svelte';

	import { createScroll } from '@/lib/utils/scroll';
	import { onMount } from 'svelte';

	let isScrolled = $state(false);

	onMount(() => {
		const handleScroll = () => {
			isScrolled = window.scrollY > 5;
		};

		handleScroll();
		window.addEventListener('scroll', handleScroll, { passive: true });
		return () => window.removeEventListener('scroll', handleScroll);
	});

	$effect(() => {
		// track page changes to re-check scroll position
		if (page && typeof window !== 'undefined') {
			isScrolled = window.scrollY > 5;
		}
	});
</script>

{#key page}
	<div class="bg-background text-foreground relative scroll-smooth" use:createScroll>
		<header
			class="sticky top-0 z-20 bg-background/85 backdrop-blur-md transition-[border-color,background-color] duration-200 {isScrolled
				? 'border-b border-border/60'
				: 'border-b border-transparent'}"
		>
			<slot name="header" />
		</header>

		<main class="min-h-[calc(100vh-4rem)]">
			<PageContent>
				<slot name="main" />
			</PageContent>
		</main>

		<slot name="footer">
			<Footer />
		</slot>
	</div>
{/key}
