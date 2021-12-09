<script>
  import { createForm } from 'svelte-forms-lib';
  import LGPD from '$lib/components/LGPD.svelte';
  import TellAFriend from '$lib/components/TellAFriend.svelte';
  import Field from './Field.svelte';
  import Targets from './TargetField.svelte';

  let success;
  export let widget = {};

  const { form, errors, handleChange, handleSubmit } = createForm({
    initialValues: {
      targets: '',
      email: '',
      first_name: '',
      last_name: '',
      state: '',
      city: '',
      subject: widget?.settings?.pressure_subject,
      body: widget?.settings?.pressure_body
    },
    validate: (values) => {
      let errs = {};
      if (!values.email) errs.email = 'Preenchimento obrigatório';
      if (!values.targets) errs.targets = 'Preenchimento obrigatório';
      if (!values.first_name) errs.first_name = 'Preenchimento obrigatório';
      return errs;
    },
    onSubmit: (values) => {
      console.log('submit', values);
      success = true;
    }
  })

  let optimization_enabled = widget?.settings?.optimization_enabled || true;
  let disabled_subject_and_body = widget?.settings?.disable_edit_field || optimization_enabled;
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
    <Targets bind:value={$form.targets} name="targets" widget={widget} />

    <Field
      bind:value={$form.email}
      bind:error={$errors.email}
      change={handleChange}
      label='E-mail'
      name='email'
      placeholder='Insira seu e-mail' />
    
    <Field
      bind:value={$form.first_name}
      bind:error={$errors.first_name}
      change={handleChange}
      label='Nome'
      name='first_name'
      placeholder='Insira seu nome' />

    <Field
      bind:value={$form.last_name}
      change={handleChange}
      label='Sobrenome'
      name='last_name'
      placeholder='Insira seu sobrenome' />

    {#if widget.settings?.show_state === 's'}
      <Field
        bind:value={$form.state}
        change={handleChange}
        label='Estado'
        name='state'
        placeholder='Selecione seu estado'
        type="select"
      >
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
      <Field
        bind:value={$form.city}
        change={handleChange}
        label='Cidade'
        name='city'
        placeholder='Insira sua cidade' />
    {/if}

    <Field
      bind:value={$form.subject}
      change={handleChange}
      name="subject"
      label="Assunto"
      disabled={disabled_subject_and_body} />
    <Field
      bind:value={$form.body}
      change={handleChange} 
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