<script lang="ts">
  import { v4 as uuidv4 } from 'uuid'
  let inputItem: string = ''
  let todos = JSON.parse(localStorage.getItem("todos") || "[]") || []
  function handleForm(e: any) {
    e.preventDefault()
    if (inputItem.trim() !== "" ){
      // console.log(inputItem.trim())
      let newItem = {
        id: uuidv4(),
        name: inputItem.trim(),
        completed: false
      }
      todos = [...todos, newItem]
      inputItem = ""
      localStorage.setItem("todos", JSON.stringify(todos))
    }
  }
  $: console.log(todos)
  function selected(e: any) {
    console.log(e.target.dataset.id)
    let id = e.target.dataset.id
    let element = document.querySelector(`#todo-${id}`)
    element?.classList.toggle("completed-false")
    element?.classList.toggle("completed-true")
    todos = todos.map((todo: any) => {
      if (todo.id === id) {
        todo.completed = !todo.completed
      }
      return todo
    })
    localStorage.setItem("todos", JSON.stringify(todos))
  }

  function handleDelete(e: any) {
    let id = e.target.dataset.id
    let todosFiltered = todos.filter((todo: any) => {
      return todo.id.toString() !== id
    })
    todos = todosFiltered
    localStorage.setItem("todos", JSON.stringify(todos))
  }
</script>

<main>
  <form on:submit={handleForm}>
    <label for="newitem">Nuevo item</label>
    <input type="text" name="newitem" bind:value={inputItem} autocomplete="off"/>
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
        {:else} 
        <div class="todo-item">
          <li>No hay nada para hacer!</li>
        </div>
      {/if}
    </ul>
  </div>
  <div class="todo-list-completed">

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
