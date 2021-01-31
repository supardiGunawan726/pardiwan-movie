<script context="module">
  import { movieDetailUrl } from "../_helper/urlGenerator.js";

  export async function preload(page, session) {
    const { id } = page.params;

    const r = await this.fetch(movieDetailUrl(id));
    const data = await r.json();

    return { data };
  }
</script>

<script>
  import Poster from "../../components/Poster.svelte";
  import Item from "../../components/Item.svelte";
  import { getImageUrl, getTrailerUrl } from "../_helper/urlGenerator.js";
  export let data;

  let releaseDate = data.release_date;
  let tagline = data.tagline;
  let heroImage = getImageUrl(data.backdrop_path);
  let posterImage = getImageUrl(data.poster_path, "w500");
  let title = data.title;
  let rate = data.vote_average;
  let voters = data.vote_count;
  let genres = [];
  let directors = [];
  let caster = [];
  let overview = data.overview;
  let trailerUrl = getTrailerUrl(data.videos.results[0].key);
  let similar = [];

  data.credits.crew.forEach(function (entry) {
    if (entry.job === "Director") {
      directors.push(entry.name);
    }
  });

  data.genres.forEach((genre) => {
    genres.push(genre.name);
  });

  let i = 0;
  data.credits.cast.forEach((item) => {
    i++;
    if (i <= 5) {
      caster.push(item.name);
    }
  });

  data.similar.results.forEach((movie) => {
    let imageUrl = getImageUrl(movie.poster_path);
    let id = movie.id;
    let title = movie.title;
    let desc = movie.overview;
    let vote = movie.vote_average;
    similar.push({imageUrl, id, title, desc, vote});
  });

  const goToid = (e) => {
    window.location = "movie/" + e.detail.id;
  }
</script>

<svelte:head>
  <title>{ title }</title>
  <link rel="preconnect" href="https://fonts.gstatic.com" />
  <link
    href="https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,400;0,600;0,700;1,400&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<section class="container">
  <div class="hero">
    <Poster imageUrl={heroImage}>
      <div class="hero__tagline">{tagline}</div>
    </Poster>
  </div>
  <div class="content__wrapper">
    <div class="content">
      <div class="info">
        <div class="info__poster">
          <Poster imageUrl={posterImage} />
        </div>
        <div>
          <h4 class="info__title">{title}</h4>
          <span class="info__release">{releaseDate}</span>
          <span class="info__genres">{genres.join(", ")}</span>
          <div class="info__rate">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="14"
              height="14"
              fill="yellow"
              viewBox="0 0 24 24"
              ><path
                d="M12 .587l3.668 7.568 8.332 1.151-6.064 5.828 1.48 8.279-7.416-3.967-7.417 3.967 1.481-8.279-6.064-5.828 8.332-1.151z"
              /></svg
            >
            <span>{rate} of {voters} voters</span>
          </div>
          <span class="info__director">Directed by {directors.join(", ")}.</span
          >
          <span class="info__caster"
            >Cast by {caster.join(", ")} and others.</span
          >
        </div>
      </div>
      <div class="overview">
        <h4>overview</h4>
        <p>{overview}</p>
      </div>
      <div class="trailer">
        <h4>Trailer</h4>
        <iframe
          title="trailer"
          width="100%"
          height="200px"
          src={trailerUrl}
          frameborder="0"
          allowfullscreen
        />
      </div>
    </div>
    <div class="sidebar">
      <h4>Similar Movies</h4>
      <div class="item__wrapper">
        {#if similar.length > 0}
          {#each similar as movie}
            <div class="item">
              <Item
                on:clicked={goToid}
                imageUrl={movie.imageUrl}
                id={movie.id}
                title={movie.title}
                desc={movie.desc}
                vote={movie.vote}
              />
            </div>
          {/each}
        {:else}
          <div class="zero">Ups there is no movies</div>
        {/if}
      </div>
    </div>
  </div>
</section>

<style>
  .container {
    width: 100%;
  }

  .hero {
    width: 100%;
    height: 200px;
    margin-bottom: 1rem;
    position: relative;
    box-shadow: inset 0 0 30px 30px #00000055;
    border-radius: 20px;
  }

  .hero__tagline {
    margin: 1rem;
    color: white;
    position: absolute;
    bottom: 0;
    font-size: 0.8rem;
    font-style: italic;
    font-family: 'Poppins', sans-serif;
  }

  .content__wrapper {
    display: grid;
    grid-template-columns: 1fr;
    grid-gap: 1rem;
  }

  .content,
  .sidebar {
    width: 100%;
    height: 100%;
  }

  .info {
    width: 100%;
    display: grid;
    grid-template-columns: 35% auto;
    grid-gap: 0.5rem;
  }

  .info__poster {
    height: 150px;
  }

  .info div:nth-child(2) {
    padding: 0.5rem 0;
  }

  .info__title {
    color: white;
    font-size: 1.2rem;
    font-family: 'Poppins', sans-serif;
    line-height: 1em;
    margin-bottom: 0.5rem;
  }

  .info__genres,
  .info__release {
    color: #eee;
    font-size: 0.7rem;
    font-style: italic;
    font-family: 'Poppins', sans-serif;
    display: block;
  }

  .info__rate {
    display: flex;
    align-items: center;
    color: yellow;
    font-weight: bold;
    font-size: 14px;
    font-family: 'Poppins', sans-serif;
  }

  .info__rate span {
    margin-left: 0.25rem;
    font-family: 'Poppins', sans-serif;
  }

  .info__director,
  .info__caster {
    display: block;
    color: #eee;
    font-size: 0.8rem;
    font-family: 'Poppins', sans-serif;
    margin-top: 0.5rem;
  }

  .info__caster {
    margin-top: 0.25rem;
  }

  .overview,
  .trailer {
    margin-top: 1rem;
  }

  .overview h4,
  .trailer h4,
  .sidebar h4 {
    color: white;
    font-size: 1.2rem;
    font-family: 'Poppins', sans-serif;
    text-transform: capitalize;
    margin-bottom: 0.25rem;
  }

  .overview p {
    font-size: 0.8rem;
    text-align: 1.2rem;
    color: #eee;
  }


  .item__wrapper {
    padding: 0.5rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .item {
    width: 100%;
    height: 170px;
  }

  @media screen and (min-width: 480px) {
    .hero {
      height: 300px;
    }

    .info__poster {
      height: 200px;
    }

  }

  @media screen and (min-width: 768px) {
    .hero {
      height: 400px;
    }

    .info {
      grid-template-columns: 30% auto;
    }

    .info__poster {
      height: 250px;
    }

    iframe {
      height: 300px;
    }

  }

  @media screen and (min-width: 1280px) {
    .hero {
      height: 500px;
    }

    .content__wrapper {
      grid-template-columns: 70% auto;
    }

    .info {
      grid-template-columns: 25% auto;
    }

    iframe {
      height: 400px;
    }

  }

  @media screen and (min-width: 1600px) {
    .hero {
      height: 600px;
    }

    .info {
      grid-template-columns: 20% auto;
    }

    iframe {
      height: 500px;
    }

  }
</style>
