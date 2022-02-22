<div class="container">
	<nav>
		<ul>
			<li><a href="/">Home</a></li>
		</ul>
	</nav>
	<slot />
</div>

<style>
	.container {
		max-width: 800px;
		margin: 10px auto;
	}
	ul {
		margin: 0;
		padding: 0;
	}
	li {
		list-style: none;
	}
</style>
