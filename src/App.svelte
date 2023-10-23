<script lang="ts">
  import { v4 as uuidv4 } from 'uuid'
  let inputItem: string = ''
  let todos = JSON.parse(localStorage.getItem("todos") || "[]") || []
  let count: number 
  countFilter()
  // $: console.log(count)


  function handleForm(e: any) {
    e.preventDefault()
    if (inputItem.trim() !== "" ){
      let newItem = {
        id: uuidv4(),
        name: inputItem.trim(),
        completed: false
      }
      todos = [...todos, newItem]
      inputItem = ""
      localStorage.setItem("todos", JSON.stringify(todos))
      countFilter()

    }
  }
  function selected(e: any) {
    let id = e.target.dataset.id
    let element = document.querySelector(`#todo-${id}`)
    element?.classList.toggle("completed-false")
    element?.classList.toggle("completed-true")
    todos = todos.map((todo: any) => {
      // sacar toString
      if (todo.id.toString() === id) {
        todo.completed = !todo.completed
      }
      return todo
    })
    localStorage.setItem("todos", JSON.stringify(todos))
    countFilter()

  }

  function handleDelete(e: any) {
    let id = e.target.dataset.id
    let todosFiltered = todos.filter((todo: any) => {
      // sacar toString
      return todo.id.toString() !== id
    })
    todos = todosFiltered
    localStorage.setItem("todos", JSON.stringify(todos))
    countFilter()
  }

  function countFilter() {
    count = todos.filter((todo : any) => todo.completed === false).length
  }
</script>

<main>
  <form on:submit={handleForm}>
    <label for="newitem">Nuevo item</label>
    <input type="text" name="newitem" bind:value={inputItem} autocomplete="off"/>
    <button>Agregar</button>
  </form>
  <div class="todo-list">
    <h2>Tareas</h2>
    <ul>
      {#if todos.length > 0}
        {#if count === 0} 
        <div class="todo-item">
          <li>No hay nada para hacer!</li>
        </div>
        {/if}
        {#each todos as todo}
          {#if !todo.completed}
          <div class="todo-item">
            <input type="checkbox" name="cb-{todo.id}" data-id={todo.id} id="" checked={todo.completed} on:click={selected}>
            <li id="todo-{todo.id}" class="completed-{todo.completed}">{todo.name}</li>
            <button data-id={todo.id} on:click={handleDelete}>Delete</button>
          </div>
          {/if}
        {/each}
        {:else} 
        <div class="todo-item">
          <li>No hay nada para hacer!</li>
        </div>
      {/if}
    </ul>
  </div>
  <div class="todo-list-completed">
    <h2>Tareas Completadas</h2>
    <ul>
      {#if todos.length > 0}
        {#each todos as todo}
          {#if todo.completed}
          <div class="todo-item">
            <input type="checkbox" name="cb-{todo.id}" data-id={todo.id} id="" checked={todo.completed} on:click={selected}>
            <li id="todo-{todo.id}" class="completed-{todo.completed}">{todo.name}</li>
            <button data-id={todo.id} on:click={handleDelete}>Delete</button>
          </div>
          {/if}
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
