<script context="module">
  import { upcomingMovieUrl, popularMovieUrl, popularTvUrl, genresUrl  } from "./_helper/urlGenerator.js";

  export async function preload(page, session) {
    const res = await this.fetch(upcomingMovieUrl(1));
    const upcomingData = await res.json();

    const res2 = await this.fetch(popularTvUrl());
    const popularTvData = await res2.json();

    const res3 = await this.fetch(genresUrl());
    const genresData = await res3.json();

    return { upcomingData, popularTvData, genresData };
  }
</script>

<script>
  import { getImageUrl } from "./_helper/urlGenerator.js";
  import Poster from "../components/Poster.svelte";
  import List from "../components/List.svelte";
  import Grid from "../components/Grid.svelte";
  import { goto } from "@sapper/app";

  export let upcomingData;
  export let popularTvData;
  export let genresData;

  let hero = {
    id: upcomingData.results[0].id,
    title: upcomingData.results[0].title,
    overview: upcomingData.results[0].overview.substring(0, 70) + "....",
    image: getImageUrl(upcomingData.results[0].backdrop_path),
  };
  let upcoming = [];
  let popularTv = [];
  let genres = [];

  upcomingData.results.forEach(result => {
    let id = result.id;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    let title = result.title; 
    upcoming.push({
      id, imageUrl, title
    });
  });

  popularTvData.results.forEach(result => {
    let id = result.id;
    let imageUrl = getImageUrl(result.poster_path, "w500");
    let title = result.name; 
    popularTv.push({
      id, imageUrl, title
    });
  })

  genresData.genres.forEach(genre => {
    let id = genre.id;
    let name = genre.name;
    genres.push({ id, name })
  })

  const heroEvent = () => {
    goto(`movie/${hero.id}`)
  }

  const upcomingEvent = (e) => {
    goto(`movie/${e.detail.id}`);
  }

  const popularTvEvent = (e) => {
    goto(`tv/${e.detail.id}`);
  }
</script>

<svelte:head>
  <title>Pardiwan Movie Update</title>
</svelte:head>

<main>
  <!-- hero -->
  <div class="hero" on:click={heroEvent}>
    <Poster imageUrl={hero.image}>
      <div class="hero__info">
        <h4 class="hero__title">{hero.title}</h4>
        <p class="hero__overview">{hero.overview}</p>
      </div>
    </Poster>
  </div>

  <!-- content -->
  <div class="content">
    <div>
      <!-- upcoming movie list -->
      <div class="list">
        <div>
          <h4 class="list__title">Upcoming movie</h4>
          <a class="list__link" href="movie/upcoming/1">see more</a>
        </div>
        <List data={upcoming} on:itemSelected={upcomingEvent}/>
      </div>

      <!-- popular TV list -->
      <div class="grid">
        <div>
          <h4 class="grid__title">Popular TV</h4>
          <a class="grid__link" href="tv/popular/1">see more</a>
        </div>
        <Grid data={popularTv} on:itemSelected={popularTvEvent} />
      </div>
    </div>

    <div>
      <!-- genres list -->
      <div class="genres">
        <h4>Genres</h4>
        {#each genres as genre}
          <div class="genre">
            <div class="genre__icon" />
            <a href={`movie/genres/${genre.id}/1`} class="genre__name">{ genre.name }</a>
          </div>
        {/each}
      </div>
    </div>
  </div>
</main>

<style>
  .content {
    max-width: 100%;
  }

  .hero {
    width: 100%;
    height: 200px;
  }

  .hero__info {
    color: white;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    align-items: flex-start;
    padding: 1rem;
    box-shadow: inset 0 0 30px 30px #00000055;
  }

  .hero__title {
    font-size: 1.5rem;
    font-weight: bold;
    text-transform: capitalize;
  }

  .hero__overview {
    font-size: 0.7rem;
    font-style: italic;
  }

  .list,
  .grid,
  .genres {
    margin-top: 1rem;
  }

  .list div,
  .grid div {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .list__title,
  .grid__title,
  .genres h4 {
    color: white;
    font-size: 1.2rem;
    text-transform: capitalize;
  }

  .list__link,
  .grid__link {
    font-size: 0.8rem;
    text-decoration: none;
    text-transform: lowercase;
    color: royalblue;
  }

  .genre {
    display: flex;
    margin: 0.5rem;
    align-items: center;
  }

  .genre__icon {
    width: 7px;
    height: 7px;
    background-color: #eee;
    margin-right: 1rem;
    clip-path: polygon(50% 0, 0 100%, 100% 100%);
    transform: rotate(90deg);
  }

  .genre__name {
    color: #eee;
    text-transform: capitalize;
    text-decoration: none
  }

  .genre__name:hover {
    color: royalblue;
  }

  @media screen and (min-width: 480px) {
    .hero {
      height: 300px;
    }
  }

  @media screen and (min-width: 768px) {
    .hero {
      height: 400px;
    }
  }

  @media screen and (min-width: 1280px) {
    .content {
      display: grid;
      grid-template-columns: 80% 20%;
      grid-gap: 2rem;
    }

    .hero {
      height: 500px;
    }
  }

  @media screen and (min-width: 1600px) {
    .hero {
      height: 600px;
    }
  }
</style>
