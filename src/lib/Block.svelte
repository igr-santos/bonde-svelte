<script>
  import Widget from "./Widget.svelte";

  export let block;

  function getBackgroundStyle (b) {
    if (b.bg_image)
      return `background:url('${b.bg_image}') no-repeat;background-size:cover;`;
    else if (b.bg_class) {
      try {
        const rgba = JSON.parse(b.bg_class);
        return `background-color:rgba(${rgba.r},${rgba.g},${rgba.b},${rgba.a});`;
      } catch (ex) {
        // Silent error because use className
        console.log("error", ex);
      }
    }
    return undefined;
  };
</script>

<section id={`block-${block.id}`} style={getBackgroundStyle(block)}>
  <div>
    {#each block.widgets as widget}
      <Widget widget={widget} />
    {/each}
  </div>
</section>

<style>
  section > div {
    width: 83.33333%;
    margin: 0 auto;
    padding: 5em 0;
  }

  section > div::after,
  section > div::before {
    content: ' ';
    display: table;
  }

  section > div::after {
    clear: both;
  }
</style>