<script>
    import { page } from '$app/stores';
    import { onMount } from 'svelte';

    let api_key = '42caba184f1a4239fc26e32b8e07cef0'
    let movieList;
    let searched = $page.params.searched;
    // https://api.themoviedb.org/3/movie/927341?api_key=42caba184f1a4239fc26e32b8e07cef0&language=en-US
    onMount(async () => {
        console.log('entrou searched: ')
        const movieSearched = await fetch(`https://api.themoviedb.org/3/movie/${searched.id}?api_key=${api_key}&language=en-US`);
        var res = await movieSearched.json();
        movieList = res;
        console.log(movieList);
    })
</script>
<section>
    <h1 class="text-xl text-white"> err </h1>
</section>