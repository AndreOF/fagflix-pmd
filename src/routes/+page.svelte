<script>
	import { goto } from '$app/navigation';
	import AutoComplete from 'simple-svelte-autocomplete';

	export let searched = {
		id: '',
		name: ''
	};
	let api_key = '42caba184f1a4239fc26e32b8e07cef0';

	let times_count = 2;

	async function getItems(query) {
		const response = await fetch(
			`https://api.themoviedb.org/3/search/movie?api_key=${api_key}&query=${query}`
		);
		const json = await response.json();
		const res = json.results;
		let autocompleteMovies = [
			{ id: res[0].id, name: res[0].original_title },
			{ id: res[1].id, name: res[1].original_title },
			{ id: res[2].id, name: res[2].original_title },
			{ id: res[3].id, name: res[3].original_title },
			{ id: res[4].id, name: res[4].original_title }
		];
		console.log(searched);
		return autocompleteMovies;
	}
	const search = (name, id) => {
		goto(`${name}/${id}`);
	};

	const test = () => {
		if (times_count !== 0) {
			times_count--;
			return;
		}
		search(searched.name, searched.id);
	};
</script>

<section>
	<div class="flex flex-col items-center justify-center">
		<div class="text-5xl text-center text-white font-semibold mb-12">
			FAG
			<div class="text-red-700 inline">FLIX</div>
		</div>
		<div class="flex rounded-md bg-white bg-opacity-20 p-1 border border-white border-opacity-30">
			<!--<input class="w-4/5 border-cyan-600 text-black" type="text" bind:value={selectedMovie} />-->
			<AutoComplete
				class="h-10 w-96 rounded-md"
				searchFunction={getItems}
				delay="200"
				required={true}
				labelFieldName="name"
				bind:selectedItem={searched}
				onChange={test}
			/>
		</div>
	</div>
</section>
