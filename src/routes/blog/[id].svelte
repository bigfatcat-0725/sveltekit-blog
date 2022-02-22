<script context="module">
	export const load = async ({ fetch, params }) => {
		const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${params.id}?_expand=user`)
		const post = await res.json()
		const user = post.user

		return {
			props: {
				post,
				user
			}
		}
	}
</script>

<script>
	export let post
	export let user
</script>

<h1>{post.title}</h1>
<p>{post.body}</p>
<p>- Written by <a sveltekit:prefetch href="{`/author/${user.id}`}">{user.name}</a></p>
