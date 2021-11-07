<script>

  import { form } from './index';

  export let initCheck = true
  export let validateOnChange = false
  export let stopAtFirstError = false
  export let stopAtFirstFieldError = false

  export let name = "";
  export let email = "";
  export let customNameValidators = []
  const loginForm = form(
    () => ({
      name: {
        value: name,
        validators: ["required", "min:6", ...customNameValidators]
      },
      email: { value: email, validators: ["required", "email"] }
    }),
    {
        initCheck,
        validateOnChange,
        stopAtFirstError,
        stopAtFirstFieldError,
    }
  );
  import Header from "./components/Header.svelte";
  import List from "./components/List.svelte";
</script>

<style>
  .container {
    width: 500px;
    background: #751212;
    padding: 1em;    
  }

  :global(.container *) {
    font-family: sans-serif;
  }

 
</style>




<form class="form">
  <label for="name">Name</label>
  <input data-testid="name" id="name" type="text" bind:value={name} />

  {#if $loginForm.fields.name.errors.includes('required')}
    <p>The name is required</p>
  {/if}

  {#if $loginForm.fields.name.errors.includes('min')}
    <p>The name should be at least 6 characters</p>
  {/if}

  {#if $loginForm.fields.name.pending}
    <p>Pending validation...</p>
  {/if}

  {#if $loginForm.fields.name.errors.includes('custom')}
    <p>Custom validator error</p>
  {/if}

  <label for="email">Email</label>
  <input
    id="email"
    type="email"
    bind:value={email}
    class:valid={$loginForm.fields.email.valid} />

  {#if $loginForm.fields.email.errors.includes('required')}
    <p>The email is required</p>
  {/if}
  {#if $loginForm.fields.email.errors.includes('email')}
    <p>The email is invalid</p>
  {/if}

  <button on:click|preventDefault={() => loginForm.validate()}>
    Validate form
  </button>
  <button disabled={!$loginForm.valid}>Post form</button>
</form>
<br>
<div class="container">
  <Header />
  <List />
</div>
