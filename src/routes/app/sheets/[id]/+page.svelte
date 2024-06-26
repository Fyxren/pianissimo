<script lang="ts">
	import type { PageData } from './$types';
	import * as Table from '$lib/components/ui/table';
	import { Button } from '$lib/components/ui/button';
	import { pb } from '$lib/pocketbase';
	import meta from '$lib/meta';
	import type { Sheet } from '$lib/types/global.interface';

	export let data: Sheet;
</script>

<svelte:head>
	<!-- Primary Meta Tags -->
	<title>Sheet >> {data.title} • {meta.title}</title>
	<meta name="title" content={`Sheet >> ${data.title} • ${meta.title}`} />

	<!-- Open Graph -->
	<meta property="og:url" content={meta.url + '/sheets/' + data.id} />
	<meta property="og:title" content={`Sheet >> ${data.title} • ${meta.title}`} />
	{data.image
		? `${pb.baseUrl}/api/files/${data.collectionId}/${data.id}/${data.image}`
		: '/images/no-file.png'}

	<!-- Twitter -->
	<meta property="twitter:url" content={meta.url + '/sheets/' + data.id} />
	<meta property="twitter:title" content={`Sheet >> ${data.title} • ${meta.title}`} />
	<meta
		property="twitter:image"
		content={data.image
			? `${pb.baseUrl}/api/files/${data.collectionId}/${data.id}/${data.image}`
			: '/images/no-file.png'}
	/>
</svelte:head>

<div class="grid gap-10 py-4 xl:grid-cols-2 xl:gap-20 xl:py-10">
	<div class="flex flex-col items-center gap-8 xl:flex-row">
		<img
			src={data.image
				? `${pb.baseUrl}/api/files/${data.collectionId}/${data.id}/${data.image}`
				: '/images/no-file.png'}
			alt={data.title}
			class="w-40 rounded-lg xl:w-56"
		/>
		<div>
			<h1 class="text-6xl font-bold">{data.title}</h1>
			{#if data.expand?.artist?.stageName}
				<Button
					href={`/app/artists/${data.expand?.artist?.id}`}
					class="px-0 text-lg font-normal text-white"
					variant="link">{data.expand?.artist?.stageName}</Button
				>
			{:else}
				<span class="text-lg italic">Unknown Artist</span>
			{/if}
		</div>
	</div>

	<Table.Root>
		<Table.Body>
			<Table.Row>
				<Table.Cell>Genre</Table.Cell>
				<Table.Cell>{data.genre}</Table.Cell>
			</Table.Row>
			<Table.Row>
				<Table.Cell>Source</Table.Cell>
				<Table.Cell>{data.source === '' ? '-' : data.source}</Table.Cell>
			</Table.Row>
			<Table.Row>
				<Table.Cell>Duo</Table.Cell>
				<Table.Cell>{data.duo ? '✅' : '❌'}</Table.Cell>
			</Table.Row>
			<Table.Row>
				<Table.Cell>Created At</Table.Cell>
				<Table.Cell>
					{Intl.DateTimeFormat('en-US', {
						day: 'numeric',
						month: 'long',
						year: 'numeric'
					}).format(new Date(data.created))}
				</Table.Cell>
			</Table.Row>
			<Table.Row>
				<Table.Cell>Last Updated</Table.Cell>
				<Table.Cell>
					{Intl.DateTimeFormat('en-US', {
						day: 'numeric',
						month: 'long',
						year: 'numeric'
					}).format(new Date(data.updated))}
				</Table.Cell>
			</Table.Row>
			<!-- NOTE: Published By refuses to get into expand... -->
			<!-- <Table.Row>
				<Table.Cell>Published By</Table.Cell>
				<Table.Cell>{data.expand?.publishedBy?.name}</Table.Cell>
			</Table.Row> -->
			<Table.Row>
				<Table.Cell>Clean Sheet</Table.Cell>
				<Table.Cell>
					{#if data.cleanSheet}
						<Button
							variant="secondary"
							size="sm"
							href={`${pb.baseUrl}/api/files/${data.collectionId}/${data.id}/${data.cleanSheet}?download=true`}
							>Download Clean Sheet</Button
						>
					{:else}
						<span class="italic">Not Available</span>
					{/if}
				</Table.Cell>
			</Table.Row>
			<Table.Row>
				<Table.Cell>Annotated Sheet</Table.Cell>
				<Table.Cell>
					{#if data.annotatedSheet}
						<Button
							variant="secondary"
							size="sm"
							href={`${pb.baseUrl}/api/files/${data.collectionId}/${data.id}/${data.annotatedSheet}?download=true`}
							>Download Annotated Sheet</Button
						>
					{:else}
						<span class="italic">Not Available</span>
					{/if}
				</Table.Cell>
			</Table.Row>
			<Table.Row>
				<Table.Cell>Note</Table.Cell>
				<Table.Cell>{data.note === '' ? '-' : data.note}</Table.Cell>
			</Table.Row>
		</Table.Body>
	</Table.Root>
</div>
