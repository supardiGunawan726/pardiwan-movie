<script context="module">
  import { searchMovieUrl } from "../../_helper/urlGenerator.js";

  export async function preload(page, session) {
    const { query } = page.params;

    let req = await this.fetch(searchMovieUrl(query));
    let data = await req.json();

    return { data, query };
  }
</script>

<script>
  import Grid from "../../../components/Grid.svelte";
  import Item from "../../../components/Item.svelte";
  import { getImageUrl } from "../../_helper/urlGenerator.js";
  import { goto } from "@sapper/app";

  export let data;
  export let query;

  let movies = [];
  data.results.forEach((result) => {
    let id = result.id;
    let title = result.title;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    let vote = result.vote_average;
    let overview = result.overview;
    movies.push({
      id,
      title,
      imageUrl,
      vote,
      overview,
    });
  });

  const goToid = (e) => {
    goto(`movie/${e.detail.id}`);
  };
</script>

<section>
  <div class="grid">
    <h5 class="grid__title">Search : {query}</h5>
    <div class="grid__items">
      {#if movies.length > 0}
        {#each movies as movie}
          <div class="grid__item">
            <Item
              on:clicked={goToid}
              imageUrl={movie.imageUrl}
              id={movie.id}
              title={movie.title}
              desc={movie.overview}
              vote={movie.vote}
            />
          </div>
        {/each}
      {:else}
        <div class="zero">Ups there is no movies</div>
      {/if}
    </div>
  </div>
</section>

<style>
  .grid {
    margin-top: 1rem;
  }

  .grid__title {
    font-size: 1.2rem;
    color: white;
  }

  .grid__items {
    padding: 0.5rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .grid__item {
    width: 100%;
    height: 170px;
  }

  .zero {
    width: 100%;
    font-size: 1rem;
    color: white;
    text-align: center;
    padding: 3rem 0;
  }

  @media screen and (min-width: 768px) {
    .grid__item {
      margin: 0 auto;
      width: 60%;
      height: 200px;
    }
  }

  @media screen and (min-width: 1280px) {
    .grid__item {
      width: 50%;
      height: 300px;
    }
  }

  @media screen and (min-width: 1600px) {
    .grid__item {
      width: 40%;
    }
  }
</style>
