<script>
  import LGPD from '$lib/components/LGPD.svelte';
  import TellAFriend from '$lib/components/TellAFriend.svelte';
  import Field from './Field.svelte';
  import Targets from './TargetField.svelte';

  export let widget = {};

  let optimization_enabled = widget?.settings?.optimization_enabled || true;
  let disabled_subject_and_body = widget?.settings?.disable_edit_field || optimization_enabled;

  // Form controllers
  let success;
  let formValues = {
    subject: widget?.settings?.pressure_subject,
    body: widget?.settings?.pressure_body
  };

  function handleSubmit() {
    console.log("submit", { formValues });
    success = true;
  }
</script>

{#if success}
  <TellAFriend />
{:else}
  <h2 style="background-color:{widget.settings?.main_color || 'rgba(0,0,0,0.25)'};">
    {widget.settings?.call_to_action || 'Clique para editar'}
  </h2>
  <form
    on:submit|preventDefault={handleSubmit}
  >
    <Targets bind:value={formValues.targets} name="targets" widget={widget} />

    <Field name='email' bind:value={formValues.email} label='E-mail' placeholder='Insira seu e-mail' />
    <Field name='first_name' bind:value={formValues.first_name} label='Nome' placeholder='Insira seu nome' />
    <Field name='last_name' bind:value={formValues.last_name} label='Sobrenome' placeholder='Insira seu sobrenome' />

    {#if widget.settings?.show_state === 's'}
      <Field name='state' bind:value={formValues.state} label='Estado' placeholder='Selecione seu estado' type="select">
        <option value="AC">Acre</option>
        <option value="AL">Alagoas</option>
        <option value="AP">Amapá</option>
        <option value="AM">Amazonas</option>
        <option value="BA">Bahia</option>
        <option value="CE">Ceará</option>
        <option value="DF">Distrito Federal</option>
        <option value="ES">Espírito Santo</option>
        <option value="GO">Goiás</option>
        <option value="MA">Maranhão</option>
        <option value="MT">Mato Grosso</option>
        <option value="MS">Mato Grosso do Sul</option>
        <option value="MG">Minas Gerais</option>
        <option value="PA">Pará</option>
        <option value="PB">Paraíba</option>
        <option value="PR">Paraná</option>
        <option value="PE">Pernambuco</option>
        <option value="PI">Piauí</option>
        <option value="RJ">Rio de Janeiro</option>
        <option value="RN">Rio Grande do Norte</option>
        <option value="RS">Rio Grande do Sul</option>
        <option value="RO">Rondônia</option>
        <option value="RR">Roraima</option>
        <option value="SC">Santa Catarina</option>
        <option value="SP">São Paulo</option>
        <option value="SE">Sergipe</option>
        <option value="TO">Tocantins</option>
        <option value="EX">Estrangeiro</option>
      </Field>
    {/if}

    {#if widget.settings?.show_city === 'city-true'}
      <Field name='city' bind:value={formValues.city} label='Cidade' placeholder='Insira sua cidade' />
    {/if}

    <Field
      bind:value={formValues.subject}
      name="subject"
      label="Assunto"
      disabled={disabled_subject_and_body} />
    <Field
      bind:value={formValues.body}
      type="textarea"
      name="body"
      label="Corpo do e-mail"
      disabled={disabled_subject_and_body} />

    <div class="spacing">
      <button type="submit" style="background-color:{widget.settings?.main_color || 'rgba(0,0,0,0.25)'};">{widget.settings?.button_text || 'Enviar'}</button>
      <LGPD dark />
    </div>
  </form>
{/if}

<style>
  h2 {
    font-family: inherit;
    color: rgb(255, 255, 255);
    display: grid;
    place-items: center;
    -webkit-box-align: center;
    padding: 1rem 2rem;
    margin: 0px;
    border-radius: 3px 3px 0px 0px;
    font-weight: 400;
    text-align: center;
  }
  form {
    background-color: white;
  }
  .spacing {
    padding: 2rem;
  }
  button {
    letter-spacing: 0px;
    color: rgb(255, 255, 255);
    text-transform: uppercase;
    border-radius: 3px;
    text-decoration: none;
    width: 100%;
    border-color: transparent;
    font-family: inherit;
    font-size: inherit;
    font-weight: 700;
    cursor: pointer;
    line-height: 1.125rem;
    height: auto;
    padding: 1rem 0px;
  }
</style>