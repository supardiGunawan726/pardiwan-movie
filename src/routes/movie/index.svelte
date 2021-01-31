<script context="module">
  import {
    upcomingMovieUrl,
    popularMovieUrl,
    topMovieUrl,
  } from "../_helper/urlGenerator.js";

  export async function preload(page, session) {
    const r = await this.fetch(topMovieUrl(1));
    const topMovieData = await r.json();

    const r1 = await this.fetch(upcomingMovieUrl(1));
    const upcomingData = await r1.json();

    const r2 = await this.fetch(popularMovieUrl(1));
    const popularData = await r2.json();

    return {
      topMovieData,
      upcomingData,
      popularData
    };
  }
</script>

<script>
  import Grid from "../../components/Grid.svelte";
  import { getImageUrl } from "../_helper/urlGenerator.js";
  import { goto } from "@sapper/app";

  export let topMovieData;
  export let upcomingData;
  export let popularData;

  // top movie
  let top = [];
  topMovieData.results.forEach((result) => {
    let id = result.id;
    let title = result.title;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    let vote = result.vote_average;
    top.push({ id, title, imageUrl, vote });
  });

  // upcoming movie
  let upcoming = [];
  upcomingData.results.forEach((result) => {
    let id = result.id;
    let title = result.title;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    let vote = result.vote_average;
    upcoming.push({ id, title, imageUrl, vote });
  });

  // popular movie
  let popular = [];
  popularData.results.forEach((result) => {
    let id = result.id;
    let title = result.title;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    let vote = result.vote_average;
    popular.push({ id, title, imageUrl, vote });
  });

  // on selected movie listener
  const goToid = (e) => {
    window.location = `movie/${e.detail.id}`
  }
</script>

<section>
  <!-- top rated movies -->
  <div class="grid">
    <div>
      <h4 class="grid__title">Top Rated Movies</h4>
      <a class="grid__link" href="movie/top/1" rel=external>see more</a>
    </div>
    <Grid data={top} on:itemSelected={goToid}/>
  </div>

  <!-- upcoming movies -->
  <div class="grid">
    <div>
      <h4 class="grid__title">upcoming Movies</h4>
      <a class="grid__link" href="movie/upcoming/1" rel=external>see more</a>
    </div>
    <Grid data={upcoming} on:itemSelected={goToid}/>
  </div>

  <!-- popular movies -->
  <div class="grid">
    <div>
      <h4 class="grid__title">popular Movies</h4>
      <a class="grid__link" href="movie/popular/1" rel=external>see more</a>
    </div>
    <Grid data={popular} on:itemSelected={goToid}/>
  </div>

</section>

<style>
  .grid div {
    display: flex;
    justify-content: space-between;
    margin-top: 1rem;
  }

  .grid__title {
    font-size: 1.2rem;
    color: white;
  }

  .grid__link {
    color: royalblue;
    font-size: 0.8rem;
    text-transform: lowercase;
    text-decoration: none;
  }
</style>
