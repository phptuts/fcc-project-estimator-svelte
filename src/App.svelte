<script>
  import Table from "./Table.svelte";
  import Form from "./Form.svelte";
</script>

<style>
  main {
    flex: 1;
    align-content: center;
    justify-content: center;
    width: 80%;
    margin: 0 auto;
  }
</style>

<main>
  <h1>Project Estimator</h1>
  <Form />

  <Table />

</main>
