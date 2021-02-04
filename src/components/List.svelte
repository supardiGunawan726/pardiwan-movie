<script>
  import Poster from "./Poster.svelte";
  import { createEventDispatcher } from "svelte"

  export let data;

  const dispatch = createEventDispatcher();
  const goToDetails = (id) => {
    dispatch("itemSelected", {
      id
    });
  }
</script>

<div class="list">
  {#each data as item}
    <div class="list__item" on:click={goToDetails(item.id)}>
      <div class="list__poster">
        <Poster imageUrl={item.imageUrl} />
      </div>
      <h4 class="list__title">{item.title}</h4>
    </div>
  {/each}
</div>

<style>
  .list {
    width: 100;
    padding: 0.5rem;
    display: flex;
    justify-content: flex-start;
    overflow-x: auto;
    grid-gap: 1rem;
    align-items: flex-start;
  }

  .list__item {
    display: flex;
    flex-direction: column;
    align-items: center;
    transition: transform ease-in-out 200ms;
  }

  .list__item:hover {
    transform: scale(1.05);
  }

  .list__poster {
    width: 120px;
    height: 160px;
  }

  .list__title {
    color: white;
    text-transform: capitalize;
    margin-top: 0.5rem;
    text-align: center;
    font-size: 0.8rem;
    font-weight: 600;
    font-family: 'Poppins', sans-serif;
  }
</style>
