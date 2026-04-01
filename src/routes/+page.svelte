<script lang="ts">
	import { onMount } from 'svelte';
	import { page } from '$app/stores';
	import { goto } from '$app/navigation';
	import { CharacterBrowser } from '$lib/features/character-browser';
	import { listCharacters, type CharacterFilters } from '$lib/core/api/rick-and-morty';

	let data: {
		characters: any[];
		total: number;
		pagination: any;
		filters: CharacterFilters;
		query: string;
		error?: string;
	} = {
		characters: [],
		total: 0,
		pagination: undefined,
		filters: {},
		query: ''
	};

	function readFilters(url: URL): CharacterFilters {
		const query = url.searchParams.get('q') ?? url.searchParams.get('name') ?? '';
		const pageParam = url.searchParams.get('page');
		const pageNum = pageParam ? Number(pageParam) : undefined;

		return {
			page: pageNum && Number.isFinite(pageNum) ? pageNum : undefined,
			name: query,
			status: url.searchParams.get('status') ?? undefined,
			species: url.searchParams.get('species') ?? undefined,
			type: url.searchParams.get('type') ?? undefined,
			gender: url.searchParams.get('gender') ?? undefined
		};
	}

	async function loadData() {
		const filters = readFilters($page.url);
		const query = filters.name ?? '';

		try {
			const result = await listCharacters(fetch, filters);
			data = {
				characters: result.characters,
				total: result.total,
				pagination: result.pagination,
				filters: result.filters,
				query
			};
		} catch (error) {
			data = {
				characters: [],
				total: 0,
				pagination: undefined,
				filters,
				query,
				error: error instanceof Error ? error.message : 'No se pudo cargar la API de Rick and Morty.'
			};
		}
	}

	onMount(() => {
		loadData();
	});

	// Reactive to URL changes
	$: if ($page.url) {
		loadData();
	}

	function updateQuery(value: string) {
		const trimmed = value.trim();
		const params = new URLSearchParams();

		if (trimmed) {
			params.set('q', trimmed);
		}

		const target = params.toString() ? `/?${params.toString()}` : '/';
		goto(target, { replaceState: true, noScroll: true });
	}
</script>

<CharacterBrowser data={data} onQueryChange={updateQuery} />
