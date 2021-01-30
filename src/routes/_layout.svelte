<script>
  import Nav from "../components/Nav.svelte";
  import Footer from "../components/Footer.svelte";

  export let segment;

</script>

<div class="container">
  <Nav {segment}/>
  <main>
    <slot/>
  </main>
  <Footer/>
</div>

<style>
  .container {
    width: 100%;
    margin: 0 auto;
    padding: 0.5rem 1rem
  }

  main {
    margin-top: 1rem;
  }

  @media screen and (min-width: 768px) {
    .container {
      width: 90%;
    }
  }
</style>
