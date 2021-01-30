<script>
  export let imageUrl;
  let imageStyle  = ``;
  if (imageUrl) {
    imageStyle = `background-image: url('${imageUrl}')`;
  }
</script>

<div class="image" style="{imageStyle}">
  <slot/>
</div>

<style>
  .image {
    background-color: black;
    background-position: center;
    background-size: cover;
    width: 100%;
    height: 100%;
    border-radius: 20px;
    overflow: hidden;
  }
</style>
