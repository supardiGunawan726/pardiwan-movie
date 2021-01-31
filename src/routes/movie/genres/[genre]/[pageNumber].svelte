<script context="module">
  import { movieByGenreUrl, genresUrl } from "../../../_helper/urlGenerator.js";

  export async function preload(page, session) {
    const { genre, pageNumber } = page.params;

    const r = await this.fetch(movieByGenreUrl(genre, pageNumber));
    const data = await r.json();

    const r2 = await this.fetch(genresUrl());
    const genres = await r2.json();
    let genresName;
    genres.genres.forEach((item) => {
      if (item.id == genre) {
        genresName = item.name;
      }
    });

    return { data, pageNumber, genresName, genre };
  }
</script>

<script>
  import { getImageUrl } from "../../../_helper/urlGenerator.js";
  import { goto } from "@sapper/app";
  import Grid from "../../../../components/Grid.svelte";

  export let data;
  export let pageNumber;
  export let genre;
  export let genresName;

  $: page = parseInt(pageNumber);

  let movies = [];
  data.results.forEach((result) => {
    let id = result.id;
    let title = result.title;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    movies.push({ id, title, imageUrl });
  });

  const goToId = (e) => {
    window.location = `movie/${e.detail.id}`
  }
</script>

<svelte:head>
  <title>{ genresName } movies</title>
  <link rel="preconnect" href="https://fonts.gstatic.com" />
  <link
    href="https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,400;0,600;0,700;1,400&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<section class="container">
  <h4 class="container__title">{genresName} movies</h4>
  <div class="info__wrapper">
    <h5 class="info__page">page {page}</h5>
    {#if data.page !== data.total_page}
      <a
        rel="external"
        href={`movie/genres/${genre}/` + (page + 1)}
        class="info__next">next</a
      >
    {/if}
  </div>
  <div class="grid">
    <Grid data={movies} on:itemSelected={goToId}/>
  </div>
</section>

<style>
  .container {
    width: 100%;
  }

  .container__title {
    color: white;
    font-size: 1.2rem;
    font-weight: bold;
    font-size: 'Poppins', sans-serif;
    text-transform: capitalize;
    text-align: center;
    width: 100%;
    letter-spacing: 0.1rem;
  }

  .info__wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 1rem;
  }

  .info__page {
    text-transform: capitalize;
    color: white;
    font-size: 1rem;
    font-size: 'Poppins', sans-serif;
  }

  .info__next {
    color: royalblue;
    text-transform: lowercase;
    text-decoration: none;
    font-size: 0.8rem;
    font-size: 'Poppins', sans-serif;
  }

  .grid {
    margin-top: 0.5rem;
  }
</style>
