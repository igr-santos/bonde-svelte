<script>
  import Form from '$lib/plugins/Form.svelte';
  import Pressure from '$lib/plugins/Pressure/index.svelte';

  export let widget;
</script>

<div class={`widget sm-col-${widget.sm_size} md-col-${widget.md_size} lg-col-${widget.lg_size}`}>
  {#if widget.kind === 'form'}
    <Form widget={widget} />
  {:else if widget.kind === 'pressure'}
    <Pressure widget={widget} />
  {:else}
    <p>{widget.kind}</p>
  {/if}
</div>

<style>
  .widget {
    box-sizing: border-box;
    float: left;
    padding: 0 1rem;
  }

  @media (min-width: 40em) {
    .sm-col-12 {
      width: 100%;
    }
  }
  @media (min-width: 52em) {
    .md-col-6 {
      width: 50%;
    }
    .md-mb0 {
      margin-bottom: 0;
    }
  }
  @media (min-width: 64em) {
    .lg-col-6 {
      width: 50%;
    }
    .lg-col-8 {
      width: 66.66667%;
    }
    .lg-col-4 {
      width: 33.33333%;
    }
    .lg-col-3 {
      width: 25%;
    }
  }
</style>