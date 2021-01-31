<script context="module">
  import { upcomingMovieUrl } from "../../_helper/urlGenerator.js";

  export async function preload(page, session) {
    const { pageNumber } = page.params;

    const r = await this.fetch(upcomingMovieUrl(pageNumber));
    const data = await r.json();

    return { pageNumber, data };
  }
</script>

<script>
  import { getImageUrl } from "../../_helper/urlGenerator.js";
  import Grid from "../../../components/Grid.svelte";
  import { goto } from "@sapper/app";

  export let pageNumber;
  export let data;

  $: page = parseInt(pageNumber);

  let top = [];
  data.results.forEach((result) => {
    let id = result.id;
    let title = result.title;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    top.push({ id, title, imageUrl });
  });

  const goToId = (e) => {
    window.location = `movie/${e.detail.id}`
  }

</script>

<section class="container">
  <h4 class="container__title">upcoming movies</h4>
  <div class="info__wrapper">
    <h5 class="info__page">page {page}</h5>
    {#if data.page !== data.total_page}
      <a rel="external" href={"movie/upcoming/" + (page + 1)} class="info__next"
        >next</a
      >
    {/if}
  </div>
  <div class="grid">
    <Grid data={top} on:itemSelected={goToId}/>
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
  }

  .info__next {
    color: royalblue;
    text-transform: lowercase;
    text-decoration: none;
    font-size: 0.8rem;
  }

  .grid {
    margin-top: 0.5rem;
  }
</style>
