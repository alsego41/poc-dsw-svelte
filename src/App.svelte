<script lang="ts">
  let inputItem: string = ''
  let todos = JSON.parse(localStorage.getItem("todos") || "[{}]") || undefined
  function handleForm(e: any) {
    e.preventDefault()
    if (inputItem.trim() !== "" ){
      console.log(inputItem.trim())
      // agregar id unica
      let newItem = {
        id: Math.round(Math.random() * 12412),
        name: inputItem.trim(),
        completed: false
      }

      todos = [...todos, newItem]
      console.log(todos)
      inputItem = ""
      localStorage.setItem("todos", JSON.stringify(todos))
    }
  }
  function selected(e: any) {
    console.log(e.target.dataset.id)
    let id = e.target.dataset.id
    let element = document.querySelector(`#todo-${id}`)
    element?.classList.toggle("completed-false")
    element?.classList.toggle("completed-true")
  }

  function handleDelete(e: any) {
    let id = e.target.dataset.id
    todos = todos.filter((todo: any) => {
      return todo.id.toString() !== id
    })
    localStorage.setItem("todos", JSON.stringify(todos))
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
          <button data-id={todo.id} on:click={handleDelete}>Delete</button>
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
