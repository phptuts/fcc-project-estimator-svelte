<script>
  import { createEventDispatcher } from "svelte";
  import materialStore from "./material-store.js";

  const dispatch = createEventDispatcher();
  let materials = [];

  materialStore.subscribe(items => {
    materials = items;
  });

  $: total = materials.reduce((prev, next) => {
    prev += next.price;
    return prev;
  }, 0);

  function edit(id, name, price) {
    dispatch("edit", { id, name, price });
  }

  const formatter = new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD"
  });

  function remove(id) {
    materialStore.remove(id);
  }
</script>

<style>
  table {
    width: 100%;
  }
  tr {
    cursor: pointer;
  }
  tr:last-of-type {
    cursor: inherit;
  }
</style>

<table class="primary">
  <thead>
    <tr>
      <th>Material</th>
      <th>Price</th>
      <th />
    </tr>
  </thead>
  <tbody>
    {#each materials as material (material.id)}
      <tr on:click={edit(material.id, material.name, material.price)}>
        <td>{material.name}</td>
        <td>{formatter.format(material.price)}</td>
        <td>
          <i
            on:click|stopPropagation={remove(material.id)}
            class="far fa-trash-alt" />
        </td>
      </tr>
    {/each}
    <tr>
      <td>Total</td>
      <td colspan="2">{formatter.format(total)}</td>
    </tr>
  </tbody>
</table>
