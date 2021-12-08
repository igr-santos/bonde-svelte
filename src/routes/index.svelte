<script context="module">
	/** @type {import('@sveltejs/kit').ErrorLoad} */
	export async function load({ fetch }) {
		const endpoint = 'https://api-graphql.staging.bonde.org/v1/graphql';
    const params = { slug: 'teste-de-widgets' };

    const res = await fetch(endpoint, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        query: `
          query ($slug: String!) {
            mobilizations(where: { slug: { _eq: $slug } }) {
              id
              name
              blocks(order_by: { position: asc }) {
                id
                name
                position
                bg_class
                bg_image
                widgets(order_by: { id: asc }) {
                  id
                  kind
                  settings
                  sm_size
                  lg_size
                  md_size
                }
              }
              community {
                name
                signature
              }
            }
          }
          `,
        variables: params,
      }),
    });

    if (res.ok) {
      const result = (await res.json());
      console.log("result", result);
      return {
        props: {
          mobilization: result.data.mobilizations[0]
        }
      }
    }

    return {
			status: res.status,
			error: new Error(`Could not load ${endpoint}`)
		};
	}
</script>

<script>
  import Block from '$lib/Block.svelte';
  import Footer from '$lib/Footer.svelte';
  import Navbar from '$lib/Navbar.svelte';

  export let mobilization;
</script>

<div class="container">
  <Navbar blocks={mobilization.blocks} />
  
  <div class="block-list">
    {#each mobilization.blocks as block}
      <Block block={block} />
    {/each}
  </div>

  <Footer community={mobilization.community} />
</div>

<style>
  .container {
    display: flex;
    flex-direction: column;
    flex-grow: 1;
    height: 100vh;
  }

  .block-list {
    flex: 1;
  }

  :global(html) {
    box-sizing: border-box;
  }

  :global(body) {
    -webkit-overflow-scrolling: touch;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-family: 'Source Sans Pro','Proxima Nova',sans-serif;
    line-height: 1.5;
    margin: 0;
    padding: 0;
    min-height: 100vh;
    max-width: 100vw;
    background-color: #f7f7f7;
  }
</style>