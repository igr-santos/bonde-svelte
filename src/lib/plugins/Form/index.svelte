<script>
  import { createForm } from 'svelte-forms-lib';
  import LGPD from '$lib/components/LGPD.svelte';
  import TellAFriend from '$lib/components/TellAFriend.svelte';

  export let widget;
  let success = false;

  const createValidationSchema = (values) => {
    let validationSchema = {};
    JSON.parse(widget.settings.fields).forEach((field) => {
      if (field.required !== 'false' && !values[field.uid]) {
        validationSchema[field.uid] = `${field.label} deve ser preenchido`
      }
    })
    return validationSchema
  }

  const { form, errors, handleChange, handleSubmit } = createForm({
    initialValues: {},
    validate: createValidationSchema,
    onSubmit: (values) => {
      console.log("submit", { values });
      success = true;
    }
  });

  console.log("widget", { widget, errors });
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
        <label for={field.uid}>{field.label}{field.required !== 'false' && '*'}</label>
        {#if field.kind === 'dropdown'}
          <select bind:value={$form[field.uid]} name={field.uid} disabled={field.disabled} on:change={handleChange}>
            {#each field.placeholder.split(';') as value}
              <option value={value.trim()}>{value.trim()}</option>
            {/each}
            <slot />
          </select>
        {:else}
          <input bind:value={$form[field.uid]} name={field.uid} disabled={field.disabled} on:change={handleChange}/>
        {/if}
        {#if $errors[field.uid]}
          <p class="error">{$errors[field.uid]}</p>
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
  div.form-control {
    margin-bottom: 1rem;
  }
  p.error {
    background-color: rgb(249, 202, 206);
    padding: 0.5rem;
    margin-bottom: 0.5rem;
    border-radius: 0px 3px 3px 0px;
    border-color: rgb(255, 65, 54);
    border-width: 8px;
    border-left-style: solid;
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