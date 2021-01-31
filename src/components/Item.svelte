<script>
  import Poster from "./Poster.svelte";
  import { createEventDispatcher } from "svelte";

  export let id;
  export let title;
  export let vote;
  export let desc;
  export let imageUrl;

  const dispatch = createEventDispatcher();

  const goToid = () => {
    dispatch("clicked", { id });
  };

  if (desc.length > 100) {
    desc = desc.substring(0, 100) + "....";
  }
</script>

<div class="item" on:click={goToid}>
  <div class="item__poster">
    <Poster {imageUrl} />
  </div>
  <div class="item__info">
    <h4 class="item__title">{title}</h4>
    <span class="item__vote">{vote} vote average</span>
    <div class="item__desc">
      {desc}
    </div>
  </div>
</div>

<style>
  .item {
    display: flex;
    width: 100%;
    height: 100%;
  }

  .item__poster {
    width: 40%;
    height: 100%;
    margin-right: 1rem;
  }

  .item__info {
    width: 60%;
    height: 100%;
    display: flex;
    flex-direction: column;
    padding: 0.25rem;
  }

  .item__title {
    font-size: 1rem;
    font-weight: bold;
    font-family: 'Poppins', sans-serif;
    text-transform: capitalize;
    color: white;
  }

  .item__vote {
    font-size: 0.8rem;
    font-family: 'Poppins', sans-serif;
    line-height: 1.5rem;
    color: yellow;
    margin-bottom: 0.5rem;
  }

  .item__desc {
    overflow: hidden;
    text-overflow: ellipsis;
    display: inline-block;
    width: 100%;
    font-size: 0.8rem;
    font-family: 'Poppins', sans-serif;
    line-height: 1.2em;
    margin: auto 0;
    color: #eee;
  }

  @media screen and (min-width: 768px) {
    .item__title {
      font-size: 1.2rem;
    }

    .item__vote, .item__desc {
      font-size: 1rem;
    }
  }

  @media screen and (min-width: 1280px) {
    .item__title {
      font-size: 1.3rem;
    }
  }

  @media screen and (min-width: 1600px) {
    .item__title {
      font-size: 1.5rem;
    }
  }

</style>
