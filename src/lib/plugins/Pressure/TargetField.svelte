<script>
  import { onMount } from 'svelte';

  export let name;
  export let value;
  export let widget;

  let targets = [];

  onMount(async () => {
    if (widget.settings?.pressure_type === 'group') {
      const endpoint = 'https://api-graphql.staging.bonde.org/v1/graphql';
      const params = { widget_id: widget.id };

      const res = await fetch(endpoint, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          query: `
            query ($widget_id: Int!) {
              pressure_targets(
                where: { widget_id: { _eq: $widget_id } }
                order_by: { label: asc }
              ) {
                identify
                label
                targets
                email_subject
                email_body
              }
            }
            `,
          variables: params,
        }),
      });
      const { data } = await res.json();

      // Define variable
      targets = data.pressure_targets;
    }
  })
</script>

<div class="target-field">
  {#if widget.settings?.pressure_type === 'group'}
    <label for={name}>Selecione os alvos</label>
    <select bind:value={value} {name}>
      <option value="">{widget.settings?.select_label || 'Selecione um grupo de alvos'}</option>
      {#each targets as target}
        <option value={target.identify}>{target.label}</option>
      {/each}
    </select>
  {/if}
  {#if !!value}
    <!-- svelte-ignore a11y-label-has-associated-control -->
    <label>Quem você vai pressionar? ({(targets.filter((t) => t.identify === value)[0]?.targets || []).length} alvos)</label>
    <div class="targets">
      {#each (targets.filter((t) => t.identify === value)[0]?.targets || []) as target}
        <div class='target'>
          <p>{(target.split('<')[0]).trim()}</p>
          <p>{(target.split('<')[1]).replace('>', '')}</p>
        </div>
      {/each}
    </div>
  {/if}
</div>

<style>
  .target-field {
    padding: 1rem 2rem;
    background-color: rgb(238, 238, 238);
  }
  .targets {
    display: flex;
    flex-direction: row;
  }
  .target {
    background-color: #fff;
    padding: 0.5rem 1rem;
    margin-right: 0.5rem;
    border-radius: 3px;
  }
  .target > p {
    font-size: 0.8rem;
    font-weight: 600;
    color: rgb(34, 34, 34);
    margin: 0px;
    display: grid;
    grid-template-rows: auto;
  }
  label {
    line-height: 1.5;
    margin-bottom: 0px;
    font-size: 0.75rem;
    font-weight: 600;
    color: rgb(86, 86, 86);
    text-transform: none;
    letter-spacing: 0px;
  }
  select {
    border: none;
    height: 2rem;
    outline: none;
    font-size: 0.9rem;
    width: 100%;
    font-family: "Nunito Sans", sans-serif;
    color: rgb(0, 0, 0);
    margin: 8px 0px;
    background: none;
  }
</style>