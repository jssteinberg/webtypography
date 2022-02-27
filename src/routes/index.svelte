<h1>Items</h1>

<ul>
	{#each items as item}
		<li>
			<a
				id={item.title.toLowerCase()}
				href={item.path}
				sveltekit:prefetch
			>
				{item.title}
			</a>
		</li>
	{/each}
</ul>

<script>
	export let items;
</script>

<script context="module">
	const mdFiles = import.meta.globEager(`./*.md`);
	const getSlug = (path) => path.replace(/.*\/([^/]*)\..*$/, "$1");

	export const load = async ({ url }) => {
		const data = Object.keys(mdFiles)
			.map((path) => {
				return {
					filePath: path,
					slug: getSlug(path),
					path: `/${getSlug(path)}`,
					title: mdFiles[path].metadata?.title || getSlug(path),
					metadata: mdFiles[path].metadata,
				};
			});

		return {
			props: {
				items: data,
			},
		};
	};
</script>
