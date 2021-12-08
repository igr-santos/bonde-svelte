<script>
  export let community;
</script>

<footer>
  <div>
    <span>Criado por {community.signature?.name || community.name} usando <strong>BONDE</strong></span>
  </div>
  <div>
    <!-- svelte-ignore a11y-invalid-attribute -->
    <a href="#">Politica de privacidade</a>
  </div>
</footer>

<style>
  footer {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    background-color: white;
    padding: 20px calc((100% - 83.33333%) / 2)
  }
</style>