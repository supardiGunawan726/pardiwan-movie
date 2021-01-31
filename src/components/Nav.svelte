<script>
  import { goto } from "@sapper/app";
  let isOpen = false;
  export let segment;

  let searchKey = "";

  const menuToggle = () => {
    isOpen = !isOpen;
  };

  const search = () => {
    goto(`movie/search/${searchKey}`);
  }
</script>

<nav>
  <div class="header">
    <a href="." class="header__brand">pardiwan</a>
    <div class="header__menu" on:click={menuToggle}>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        height="24"
        viewBox="0 0 24 24"
        fill="white"
        width="24">
        {#if isOpen}
          <path d="M0 0h24v24H0z" fill="none" />
          <path
            d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"
          />
        {:else}
          <path d="M0 0h24v24H0V0z" fill="none" />
          <path
            d="M4 18h16c.55 0 1-.45 1-1s-.45-1-1-1H4c-.55 0-1 .45-1 1s.45 1 1 1zm0-5h16c.55 0 1-.45 1-1s-.45-1-1-1H4c-.55 0-1 .45-1 1s.45 1 1 1zM3 7c0 .55.45 1 1 1h16c.55 0 1-.45 1-1s-.45-1-1-1H4c-.55 0-1 .45-1 1z"
          />
        {/if}
      </svg>
    </div>
  </div>
  <div class="links" class:hidden={isOpen === false}>
    <a rel=external class:active={segment === undefined} href="." class="links__item">home</a>
    <a rel=external class:active={segment === "movie"} href="movie" class="links__item">Movie</a>
    <a rel=external class:active={segment === "tv"} href="tv" class="links__item">TV Series</a>
  </div>
  <div class="search">
    <form on:submit|preventDefault={search}>
      <input bind:value={searchKey} placeholder="Search movie..." type="text" class="search__input" required/>
      <button class="search__button">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          height="24"
          viewBox="0 0 24 24"
          width="24"
          fill="black">
          <path d="M0 0h24v24H0z" fill="none" />
          <path
            d="M15.5 14h-.79l-.28-.27C15.41 12.59 16 11.11 16 9.5 16 5.91 13.09 3 9.5 3S3 5.91 3 9.5 5.91 16 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"
          />
        </svg>
      </button>
    </form>
  </div>
</nav>

<style>
  nav {
    width: 100%;
    display: flex;
    flex-direction: column;
  }

  .header {
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .header__brand {
    color: crimson;
    font-size: 1.5rem;
    font-weight: bold;
    text-decoration: none;
    text-transform: uppercase;
    letter-spacing: 3px;
  }

  .header__menu {
    width: 30px;
    height: 30px;
    display: flex;
    align-items: center;
  }

  .links {
    display: flex;
    flex-direction: column;
    margin-top: 0.5rem;
  }

  .links__item {
    margin: 0 -1rem;
    display: block;
    padding: 1rem;
    color: white;
    font-weight: 600;
    text-transform: capitalize;
    text-decoration: none;
    transition: all ease-out 300ms;
  }

  .links__item:hover {
    background-color: crimson;
    color: white;
  }

  .hidden {
    display: none;
  }

  .search {
    margin-top: 0.5rem;
  }

  .search form {
    background-color: white;
    display: flex;
    align-items: center;
    border-radius: 25px;
    overflow: hidden;
  }

  .search__input {
    height: 100%;
    padding: 0.7rem 1rem;
    outline: none;
    flex-grow: 2;
    border: none;
  }

  .search__button {
    outline: none;
    border: none;
    background-color: white;
    padding: 0 0.5rem;
  }

  .active {
    background-color: crimson;
    color: white;
  }

  @media screen and (min-width: 1280px) {
    nav {
      flex-direction: row;
      justify-content: flex-start;
      align-items: center;
    }

    .header {
      flex-grow: 0;
      width: auto;
      align-items: center;
    }

    .header__menu {
      display: none;
    }

    .links {
      display: flex;
      flex-direction: row;
      justify-content: space-around;
      align-items: center;
      gap: 0.5rem;
      flex-grow: 1;
      margin: 0 1rem;
    }

    .links__item {
      padding: 0.5rem 1rem;
      border-radius: 20px;
    }

    .search {
      flex-grow: 2;
    }
  }
</style>
