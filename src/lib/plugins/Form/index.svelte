<script>
  import LGPD from '$lib/components/LGPD.svelte';
  import TellAFriend from '$lib/components/TellAFriend.svelte';

  export let widget;
  
  let success;
  let formValues = {};

  function handleSubmit() {
    console.log("submit", { formValues });
    success = true;
  }
</script>

{#if success}
  <TellAFriend />
{:else}
  <form
    on:submit|preventDefault={handleSubmit}
    style="background-color:{widget.settings?.main_color || 'rgba(0,0,0,0.25)'};"
  >
    <h2>{widget.settings?.call_to_action || "Clique para editar"}</h2>
    {#each JSON.parse(widget.settings.fields) as field}
      <div class="form-control">
        {#if field.kind === 'dropdown'}
          <label for={field.uid}>{field.label}{field.required !== "false" ? '*' : ''}</label>
          <select name={field.uid} bind:value={formValues[field.uid]}>
            {#each field.placeholder.split(';') as value}
              <option value={value.trim()}>{value.trim()}</option>
            {/each}
          </select>
        {:else}
          <label for={field.uid}>{field.label}{field.required !== "false" ? '*' : ''}</label>
          <input name={field.uid} bind:value={formValues[field.uid]} />
        {/if}
      </div>
    {/each}
    
    <button type="submit">{widget.settings?.button_text || 'Enviar'}</button>
    
    <LGPD />
  </form>
{/if}

<style>
  h2 {
    color: #ffffff;
    text-align: center;
    margin: 0 0 2rem 0;
    font-weight: normal;
  }
  form {
    padding: 2rem;
    border-radius: 3px;
  }
  .form-control {
    margin-bottom: 1rem;
  }
  label {
    cursor: pointer;
    font-size: 0.75rem;
    font-weight: 600;
    letter-spacing: 0;
    text-transform: uppercase;
    color: #fff;
  }
  input, select {
    cursor: pointer;
    border-radius: 2px;
    padding: 1rem;
    box-sizing: border-box;
    height: auto;
    border: 1px solid #eee;
    font-size: inherit;
    font-family: inherit;
    width: 100%;
    background-color: white;
  }
  button {
    background-color: rgba(0, 0, 0, 0.5);
    color: #ffffff;
    padding: 1rem;
    width: 100%;
    border-radius: 3px;
    border: 1px solid transparent;
    text-transform: uppercase;
    letter-spacing: 0;
    font-family: inherit;
    font-size: inherit;
    font-weight: bold;
    line-height: 1.125rem;
    -webkit-text-decoration: none;
    text-decoration: none;
    cursor: pointer;
  }
</style>