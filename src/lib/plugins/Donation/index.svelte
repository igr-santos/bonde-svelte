<script>
  import LGPD from '$lib/components/LGPD.svelte';
  import TellAFriend from '$lib/components/TellAFriend.svelte';
  import Unique from './Unique.svelte';
  import Recurring from './Recurring.svelte';

  export let widget = {};
  
  function rgba (color, alpha) {
    const value = color.replace('#', '');
    let r = parseInt(value.substring(0, 2), 16);
    let g = parseInt(value.substring(2, 4), 16);
    let b = parseInt(value.substring(4, 6), 16);

    return 'rgba(' + r + ',' + g + ',' + b + ',' + alpha / 100 + ')';
  }

  let recurring_label = {
    30: 'mês',
    180: 'semestre',
    365: 'ano'
  }[widget.settings?.recurring_period]

  // Form submision controller
  let success;
  let formValues = {
    selected_value: parseInt(widget.settings?.default_donation_value || '1'),
    payment_type: widget.settings?.payment_type !== 'users_choice' ? widget.settings?.payment_type : 'recurring'
  };

  function handleSubmit() {
    console.log("submit", { formValues });
    success = true;
  }
</script>

{#if success}
  <TellAFriend />
{:else}
  <form
    on:submit|preventDefault={handleSubmit}>
    <h2 style="background-color:{widget.settings?.main_color || 'rgba(0,0,0,0.25)'};">{widget.settings?.call_to_action || 'Clique para editar'}</h2>
    <div class="spacing">

      {#if widget.settings?.payment_type === 'users_choice'}
        <div class='payment-type-field'>
          <button
            style={formValues.payment_type === 'recurring' && `color: ${widget.settings?.main_color};`}
            class:opacity={formValues.payment_type !== 'recurring'}
            on:click={() => formValues.payment_type = 'recurring'}
            type='button'>
            <Recurring />
            <span>{`Doar todo ${recurring_label}`}</span>
          </button>
          <button
            style={formValues.payment_type === 'unique' && `color: ${widget.settings?.main_color};`}
            class:opacity={formValues.payment_type !== 'unique'}
            on:click={() => formValues.payment_type = 'unique'}
            type='button'>
            <Unique />
            <span>{`Doação única`}</span>
          </button>
        </div>
      {/if}

      {#if widget.settings?.donation_value1}
        <button
          on:click={() => formValues.selected_value = 1}
          type="button"
          style={formValues.selected_value === 1 && `background-color:${rgba(widget.settings?.main_color, 35)};color:${widget.settings?.main_color}`}
        >
          {`R$`}
          {widget.settings.donation_value1}
          {formValues.payment_type === 'unique' ? '' :  '/' + recurring_label}
        </button>
      {/if}

      {#if widget.settings?.donation_value2}
        <button
          on:click={() => formValues.selected_value = 2}
          type="button"
          style={formValues.selected_value === 2 && `background-color:${rgba(widget.settings?.main_color, 35)};color:${widget.settings?.main_color}`}
        >
          {`R$`}
          {widget.settings.donation_value2}
          {formValues.payment_type === 'unique' ? '' :  '/' + recurring_label}
        </button>
      {/if}

      {#if widget.settings?.donation_value3}
        <button
          on:click={() => formValues.selected_value = 3}
          type="button"
          style={formValues.selected_value === 3 && `background-color:${rgba(widget.settings?.main_color, 35)};color:${widget.settings?.main_color}`}
        >
          {`R$`}
          {widget.settings.donation_value3}
          {formValues.payment_type === 'unique' ? '' :  '/' + recurring_label}
        </button>
      {/if}

      {#if widget.settings?.donation_value4}
        <button
          on:click={() => formValues.selected_value = 4}
          type="button"
          style={formValues.selected_value === 4 && `background-color:${rgba(widget.settings?.main_color, 35)};color:${widget.settings?.main_color}`}
        >
          {`R$`}
          {widget.settings.donation_value4}
          {formValues.payment_type === 'unique' ? '' : '/' + recurring_label}
        </button>
      {/if}

      {#if widget.settings?.donation_value5}
        <button
          on:click={() => formValues.selected_value = 5}
          type="button"
          style={formValues.selected_value === 5 && `background-color:${rgba(widget.settings?.main_color, 35)};color:${widget.settings?.main_color}`}
        >
          {`R$`}
          {widget.settings.donation_value5}
          {formValues.payment_type === 'unique' ? '' :  '/' + recurring_label}
        </button>
      {/if}

      <button type="submit" style="background-color:{widget.settings?.main_color || 'rgba(0,0,0,0.25)'};">
        {widget.settings?.button_text || 'Doar'}
      </button>
      <LGPD dark />
    </div>
  </form>
{/if}

<style>
  .spacing {
    padding: 1rem 2rem;
  }
  form {
    background-color: white;
  }
  h2 {
    padding: 1rem;
    color: #fff;
    font-weight: 400;
    margin: 0;
    text-align: center;
  }

  .payment-type-field {
    display: flex;
    flex-direction: row;
  }
  .payment-type-field > button {
    background: none;
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .payment-type-field > button > span {
    margin-top: 1rem;
  }

  .payment-type-field > button.opacity {
    opacity: 0.3;
  }

  button {
    border-radius: 0.3rem;
    box-sizing: border-box;
    border-width: 0.3rem;
    border-style: solid;
    border-color: #fff;
    cursor: pointer;
    width: 100%;
    font-weight: bold;
    opacity: 0.8;
    padding: 0.5rem 0;
    margin-bottom: 0.5rem;
    color: #b0b0b0;
    background: rgba(0, 0, 0, 0.0625);
  }

  button[type=submit] {
    width: 100%;
    padding: 1rem;
    color: #fff;
    text-transform: uppercase;
    font-weight: bold;
    border: none;
    border-radius: 3px;
    margin-top: 0.5rem;
    cursor: pointer;
  }
</style>