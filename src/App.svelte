<script lang="ts">
  function handleForm(e: any) {
    e.preventDefault()
    console.log(e)
  }
</script>

<main>
  <form on:submit={handleForm}>
    <label for="newitem">Nuevo item</label>
    <input type="text" name="newitem"/>
    <button>Agregar</button>
  </form>
</main>

<style>

</style>
