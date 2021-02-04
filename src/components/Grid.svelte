<script>
  import Poster from "./Poster.svelte";
  import { createEventDispatcher } from "svelte";

  export let data;

  const dispatch = createEventDispatcher();
  const goToDetail = (id) => {
    dispatch("itemSelected", {
      id,
    });
  };
</script>

<div class="grid">
  {#each data as item}
    <div class="grid__item" on:click={goToDetail(item.id)}>
      <div class="grid__poster">
        <Poster imageUrl={item.imageUrl} />
      </div>
      {#if item.title.length > 10}
        <h4 class="grid__title">{item.title.substring(0, 10)}...</h4>
      {:else}
        <h4 class="grid__title">{item.title}</h4>
      {/if}
    </div>
  {/each}
</div>

<style>
  .grid {
    width: 100%;
    padding: 0.5rem;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    grid-auto-rows: 180px;
    grid-gap: 1rem;
  }

  .grid__item {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    transition: transform ease-in-out 200ms;
  }

  .grid__item:hover {
    transform: scale(1.1);
  }

  .grid__poster {
    width: 100%;
    height: 100%;
  }

  .grid__title {
    text-align: center;
    color: white;
    font-size: 0.8rem;
    font-weight: 600;
    font-family: "Poppins", sans-serif;
    text-transform: capitalize;
    margin-top: 0.5rem;
  }

  @media screen and (min-width: 480px) {
    .grid {
      grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
      grid-auto-rows: 200px;
    }
  }

  @media screen and (min-width: 768px) {
    .grid {
      grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
      grid-auto-rows: 220px;
    }
  }

  @media screen and (min-width: 1280px) {
    .grid {
      grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
      grid-auto-rows: 240px;
    }
  }

  @media screen and (min-width: 1600px) {
    .grid {
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      grid-auto-rows: 260px;
    }
  }

</style>
