<script context="module">
	export const load = async ({ fetch }) => {
		const res = await fetch('/api/posts.json')
		const posts = await res.json()
		return {
			props: {
				posts
			}
		}
	}
</script>

<script>
	import { paginate, LightPaginationNav } from 'svelte-paginate'
	export let posts

	let searchTerm = ''

	$: searchedPosts = posts.filter((post) => {
		return post.title.includes(searchTerm) || post.body.includes(searchTerm)
	})

	let items = posts.reverse()

	$: searchTerm ? (items = searchedPosts) : (items = posts)

	let currentPage = 1
	let pageSize = 6
	$: paginatedItems = paginate({ items, pageSize, currentPage })
</script>

<h1>Posts</h1>

<input type="text" placeholder="search" bind:value="{searchTerm}" />

<div class="posts">
	{#if !searchTerm}
		{#each paginatedItems as item}
			<div class="post">
				<h2>{item.title.substring(0, 20)}..</h2>
				<p>{item.body.substring(0, 80)}..</p>
				<p class="link"><a sveltekit:prefetch href="{`/blog/${item.id}`}">Read More</a></p>
			</div>
		{/each}
	{:else if searchedPosts.length}
		{#each paginatedItems as item}
			<div class="post">
				<h2>{item.title.substring(0, 20)}..</h2>
				<p>{item.body.substring(0, 80)}..</p>
				<p class="link"><a sveltekit:prefetch href="{`/blog/${item.id}`}">Read More</a></p>
			</div>
		{/each}
	{:else}
		<p>No posts found with "{searchTerm}"</p>
	{/if}
</div>

<LightPaginationNav
	totalItems="{items.length}"
	pageSize="{pageSize}"
	currentPage="{currentPage}"
	limit="{1}"
	showStepOptions="{true}"
	on:setPage="{(e) => (currentPage = e.detail.page)}"
/>

<style>
	.posts {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-gap: 20px;
		margin: 10px 0;
	}
	.post {
		padding: 20px;
		border: 1px solid #ddd;
		box-shadow: 0 0 10px #eee;
	}
	h2 {
		margin: 0;
	}
	.link {
		text-align: right;
	}
	input {
		border: 1px solid #ddd;
		padding: 10px 20px;
		border-radius: 5px;
	}

	@media screen and (max-width: 600px) {
		.posts {
			display: grid;
			grid-template-columns: 1fr;
			grid-gap: 20px;
			margin: 30px 0;
		}
	}
</style>
