<script>
  export let blocks;
</script>

<nav>
  <ul>
    {#each blocks as block}
      <li>
        <a href={`#block-${block.id}`}>{block.name || `Block ${block.position}`}</a>
      </li>
    {/each}
  </ul>
</nav>

<style>
  nav {
    position: fixed;
    width: 100vw;
    background-color: rgba(0, 0, 0, 0.5);
  }

  ul {
    display: flex;
    flex-direction: row;
    list-style-type: none;
    justify-content: center;
    padding: 0;
    margin: 0;
  }

  li {
    display: flex;
  }

  a {
    color: white;
    font-weight: 700;
    text-decoration: none;
    line-height: 1.125rem;
    font-size: 16px;
    padding: 1rem;
  }
</style>