<script lang="ts">
  let inputItem: string = ''
  let todos = JSON.parse(localStorage.getItem("todos") || "[{}]") || undefined
  function handleForm(e: any) {
    e.preventDefault()
    console.log(inputItem)
    let newItem = {
      id: Math.round(Math.random() * 12412),
      name: inputItem,
      completed: false
    }
    todos = [...todos, newItem]
    console.log(todos)
    inputItem = ""
    localStorage.setItem("todos", JSON.stringify(todos))
  }
  function selected(e: any) {
    console.log(e.target.dataset.id)
    let id = e.target.dataset.id
    let element = document.querySelector(`#todo-${id}`)
    element?.classList.toggle("completed-false")
    element?.classList.toggle("completed-true")
  }
</script>

<main>
  <form on:submit={handleForm}>
    <label for="newitem">Nuevo item</label>
    <input type="text" name="newitem" bind:value={inputItem} />
    <button>Agregar</button>
  </form>
  <div class="todo-list">
    <ul>
      {#if todos.length > 0}
      {#each todos as todo}
      <div class="todo-item">
        <input type="checkbox" name="cb-{todo.id}" data-id={todo.id} id="" checked={todo.completed} on:click={selected}>
        <li id="todo-{todo.id}" class="completed-{todo.completed}">{todo.name}</li>
      </div>
      {/each}
      {/if}
    </ul>
  </div>
</main>

<style>
  ul {
    list-style: none;
  }
  .todo-item {
    display: flex;
    flex-direction: row;
  }
  .completed-true {
    color: green;
  }
  .completed-false {
    color: orange;
  }
</style>
