<script>
  export let type = 'text';
  export let name;
  export let change;
  export let error = undefined;
  export let label;
  export let value;
  export let placeholder = undefined;
  export let disabled = false;
</script>

<div class="form-control">
  <div class="inline-flex">
    <label for={name}>{label}</label>
    {#if error}
      <span class="error">{error}</span>
    {/if}
  </div>
  {#if type === 'select'}
    <select bind:value={value} on:change={change} {name} {disabled}>
      {#if placeholder}
        <option>{placeholder}</option>
      {/if}
      <slot />
    </select>
  {:else if type === 'textarea'}
    <textarea bind:value={value} on:change={change} {name} {placeholder} {disabled} />
  {:else}
    <input bind:value={value} on:change={change} {name} {placeholder} {disabled} />
  {/if}
</div>

<style>
  div.form-control {
    padding: 1rem 2rem 0.5rem;
    border-bottom: 1px solid rgb(238, 238, 238);
  }
  div.inline-flex {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }
  span.error {
    font-family: "Nunito Sans", sans-serif;
    font-size: 11px;
    font-weight: 600;
    line-height: 1.36;
    letter-spacing: 0.4px;
    text-transform: uppercase;
    color: rgb(255, 9, 49);
  }
  label {
    line-height: 1.5;
    margin-bottom: 0px;
    font-size: 0.75rem;
    font-weight: 600;
    color: rgb(170, 170, 170);
    text-transform: none;
    letter-spacing: 0px;
  }
  input, select, textarea {
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
  textarea {
    height: 7rem;
  }
  input:disabled, textarea:disabled {
    color: rgb(84, 84, 84);
    cursor: not-allowed;
    background-color: rgb(235, 235, 228);
  }
</style>