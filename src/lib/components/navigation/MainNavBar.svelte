<script lang="ts">
	import routes from '@/lib/data/routes';
	import PageContent from '../layout/PageContent.svelte';
	import { twMerge } from 'tailwind-merge';

	import { page } from '$app/state';
	import { Button } from '../ui/button';
	import ModeSwitcher from '../layout/ModeSwitcher.svelte';

	const pathname = $derived(page.url.pathname);

	function isCurrentPage(href: string) {
		if (href === '/') return pathname === '/';
		return pathname === href || pathname.startsWith(`${href}/`);
	}
</script>

<PageContent>
	<div class="flex w-full max-w-full items-center justify-between gap-4 px-2 py-8 md:px-8">
		<nav class="flex flex-1 gap-4">
			{#each routes.mainNav as { name, href }}
				{@const active = isCurrentPage(href)}
				<a {href} class="relative p-2 font-semibold">
					<span
						class={twMerge(
							'transition-colors',
							active ? 'text-primary' : 'text-muted-foreground hover:text-foreground'
						)}
					>
						{name}
					</span>

					{#if active}
						<span
							id="current-tab-indicator"
							class={twMerge('bg-primary absolute bottom-0 left-0 h-[4px] w-full rounded-full')}
						>
						</span>
					{/if}
				</a>
			{/each}
		</nav>
		<Button href="mailto:hrandiga126@gmail.com" class="hidden md:block">Reach out</Button>
		<ModeSwitcher />
	</div>
</PageContent>

<style>
	nav {
		view-transition-name: nav;
	}

	#current-tab-indicator {
		view-transition-name: current-tab-indicator;
	}
</style>
