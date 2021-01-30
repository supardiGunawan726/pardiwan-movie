<script context="module">
  import {
    topTvUrl,
    popularTvUrl,
  } from "../_helper/urlGenerator.js";

  export async function preload(page, session) {
    const r = await this.fetch(topTvUrl(1));
    const topTvData = await r.json();

    const r2 = await this.fetch(popularTvUrl(1));
    const popularTvData = await r2.json();
    console.log(popularTvData);

    return {
      topTvData,
      popularTvData
    };
  }
</script>

<script>
  import Grid from "../../components/Grid.svelte";
  import { getImageUrl } from "../_helper/urlGenerator.js";
  import { goto } from "@sapper/app";

  export let topTvData;
  export let popularTvData;

  // top tv
  let top = [];
  topTvData.results.forEach((result) => {
    let id = result.id;
    let title = result.name;
    let vote = result.vote_average;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    if (result.poster_path) {
      top.push({ id, title, imageUrl, vote });
    }
  });

  // popular tv
  let popular = [];
  popularTvData.results.forEach((result) => {
    let id = result.id;
    let title = result.name;
    let vote = result.vote_average;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    if (result.poster_path) {
      popular.push({ id, title, imageUrl, vote });
    }
  });


  const goToid = (e) => {
    goto(`tv/${e.detail.id}`);
  };
</script>

<section>

  <!-- top rated tv -->
  <div class="grid">
    <div>
      <h4 class="grid__title">Top Rated TV</h4>
      <a class="grid__link" href="tv/top/1">see more</a>
    </div>
    <Grid data={top} on:itemSelected={goToid} />
  </div>

  <!-- popular tv -->
  <div class="grid">
    <div>
      <h4 class="grid__title">Popular TV</h4>
      <a class="grid__link" href="tv/popular/1">see more</a>
    </div>
    <Grid data={popular} on:itemSelected={goToid} />
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
