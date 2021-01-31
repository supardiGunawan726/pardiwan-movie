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
      {#if (item.title.length > 10)}
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
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
  }

  .grid__item {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    width: 90px;
    transition: transform ease-in-out 200ms;
  }

  .grid__item:hover {
    transform: scale(1.1);
  }

  .grid__poster {
    width: 100%;
    min-height: 120px;
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
</style>
