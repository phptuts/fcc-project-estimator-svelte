<script>
  import materialStore from "./material-store.js";
  export let id;
  export let name = "";
  export let price = 5;

  $: mode = id ? "edit" : "add";
  $: canSubmit = price >= 0 && name !== "";
  function submit() {
    if (!canSubmit) {
      return;
    }

    if (mode === "add") {
      materialStore.add(name, price);
    }

    if (mode === "edit") {
      materialStore.edit(id, name, price);
    }

    name = "";
    price = 5;
    id = undefined;
  }

  function cancel() {
    price = 5;
    name = "";
    id = undefined;
  }
</script>

<style>
  button {
    margin-left: 20px;
  }
  button:disabled {
    cursor: not-allowed;
  }
</style>

<form on:submit|preventDefault={submit}>
  <fieldset>
    <label for="nameField">Material</label>
    <input
      bind:value={name}
      placeholder="Wood, Glue, Etc"
      type="text"
      id="nameField" />

    <label for="priceField">Price</label>
    <input
      bind:value={price}
      min="0"
      step="any"
      placeholder="Price"
      type="number"
      id="priceField" />

  </fieldset>

  <button disabled={!canSubmit} class="float-right" type="submit">
    {mode}
  </button>
  {#if mode === 'edit'}
    <button on:click={cancel} class="float-right" type="button">Cancel</button>
  {/if}
</form>
