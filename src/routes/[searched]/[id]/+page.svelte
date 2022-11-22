<script>
	import { page } from '$app/stores';
	import { onMount } from 'svelte';
	let api_key = '42caba184f1a4239fc26e32b8e07cef0';
	const movie = {
		name: '',
		year: '',
		poster: ''
	};
	let recommedations = [];
	onMount(async () => {
		var id = $page.params.id;
		const selectedMovie = await fetch(
			`https://api.themoviedb.org/3/movie/${id}?api_key=${api_key}&language=en-US`
		);
		var sel = await selectedMovie.json();
		const recommendedMovies = await fetch(
			`https://api.themoviedb.org/3/movie/${id}/recommendations?api_key=${api_key}&language=en-US&page=1`
		);
		var rec = await recommendedMovies.json();
		recommedations = rec.results;
		console.log('entrou id: ' + id);
		console.log('res: ', sel);
		movie.name = sel.original_title;
		movie.year = sel.release_date.substring(0, sel.release_date.indexOf('-'));
		movie.poster = `http://image.tmdb.org/t/p/w500${sel.poster_path}`;
		console.log('rec: ' + recommedations);
	});
</script>

<div>
	<button class="text-white">VOLTAR</button>
	<div class="flex flex-col place-items-center">
		<h2 class="text-white">Filme selecionado</h2>
		<div class="card w-52 bg-base-100 shadow-xl h-auto">
			<figure><img src={movie.poster} alt="movie poster" /></figure>
			<div class="card-body">
				<h6 class="card-title">
					<p class="text-sm">{movie.name}</p>
					<div class="badge badge-primary bg-red-700">{movie.year}</div>
				</h6>
			</div>
		</div>
	</div>
	<div class="flex flex-col">
		<h2 class="text-white place-self-center">Filmes recomendados</h2>
		<div class="grid grid-cols-4">
			{#each recommedations as r}
				<div class="text-white mt-4">
					<div class="card w-52 bg-base-100 shadow-xl">
						<figure>
							<img src={'http://image.tmdb.org/t/p/w500' + r.poster_path} alt="movie poster" />
						</figure>
						<div class="card-body">
							<h6 class="card-title">
								<p class="text-sm text-black">{r.original_title}</p>
								<div class="badge badge-primary bg-red-700">{r.release_date.substring(0, 4)}</div>
							</h6>
						</div>
					</div>
				</div>
			{/each}
		</div>
	</div>
</div>
