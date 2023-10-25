<script lang="ts">
  import { v4 as uuidv4 } from 'uuid'
  let inputItem: string = ''
  let todos = JSON.parse(localStorage.getItem("todos") || "[]") || []
  let count: number 
  let editableTodoID : string = ""
  let editInput: string = ""
  countFilter()

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

  function toggleSelection(id: string) {
    todos = todos.map((todo: any) => {
      if (todo.id === id) {
        todo.completed = !todo.completed
      }
      return todo
    }) 
    localStorage.setItem("todos", JSON.stringify(todos))
    countFilter()
  }

  function handleDelete(id: string) {
    let todosFiltered = todos.filter((todo: any) => {
      return todo.id !== id
    })
    todos = todosFiltered
    localStorage.setItem("todos", JSON.stringify(todos))
    countFilter()
  }

  function handleEdit(id: string) {
    editableTodoID = id
    todos.filter((todo: any) => {
      if (todo.id === id) {
        editInput = todo.name
      }
    })
  }

  function updateTodo(id: string) {
    todos = todos.map((todo: any) => {
      if (todo.id === id) {
        todo.name = editInput
      }
      return todo
    }) 
    localStorage.setItem("todos", JSON.stringify(todos))
    countFilter()
    editInput = ""
    editableTodoID = ""
  }

  function countFilter() {
    count = todos.filter((todo : any) => todo.completed === false).length
  }
</script>

<main>
  <form on:submit={handleForm}>
    <h2>Agregar Tarea</h2>
    <div class="todo-new">
      <input type="text" name="newitem" bind:value={inputItem} autocomplete="off" class="user-input" autofocus={true}/>
      <button>
        <img src="./src/assets/plus-solid.svg" alt="Boton agregar" class="add-img">    
      </button>
    </div>
    <!-- <label for="newitem">Nuevo item</label> -->
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

            {#if editableTodoID === todo.id}
            <input type="checkbox" name="todo-{todo.id}" bind:checked={todo.completed} on:click={() => toggleSelection(todo.id)} />
            <input type="text" name="newitem" bind:value={editInput} autocomplete="off" class="user-input" autofocus={true} 
            on:blur={() => updateTodo(todo.id)}/>
            <button on:click={() => updateTodo(todo.id)}>
              <img src="./src/assets/floppy-disk-solid.svg" alt="Boton guardar" class="save-img" >    
            </button>

            {:else}
            <input type="checkbox" name="todo-{todo.id}" bind:checked={todo.completed} on:click={() => toggleSelection(todo.id)} />
            <li id={todo.id} class="completed-{todo.completed}">{todo.name}</li>
            <button on:click={() => handleEdit(todo.id)}>
              <img src="./src/assets/pencil-solid.svg" alt="Boton editar" class="edit-img">    
            </button>
            <button on:click={() => handleDelete(todo.id)}>
              <img src="./src/assets/trash-solid.svg" alt="Boton borrar" class="del-img">    
            </button>
            {/if}
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
            {#if editableTodoID === todo.id}
            <input type="checkbox" name="todo-{todo.id}" bind:checked={todo.completed} on:click={() => toggleSelection(todo.id)} />
            <input type="text" name="newitem" bind:value={editInput} autocomplete="off" class="user-input" autofocus={true} 
            on:blur={() => updateTodo(todo.id)}/>
            <button on:click={() => updateTodo(todo.id)}>
              <img src="./src/assets/floppy-disk-solid.svg" alt="Boton guardar" class="save-img" >    
            </button>

            {:else}
            <input type="checkbox" name="todo-{todo.id}" bind:checked={todo.completed} on:click={() => toggleSelection(todo.id)} />
            <li id={todo.id} class="completed-{todo.completed}">{todo.name}</li>
            <button on:click={() => handleEdit(todo.id)}>
              <img src="./src/assets/pencil-solid.svg" alt="Boton editar" class="edit-img">    
            </button>
            <button on:click={() => handleDelete(todo.id)}>
              <img src="./src/assets/trash-solid.svg" alt="Boton borrar" class="del-img">    
            </button>
            {/if}
          </div>
          {/if}
        {/each}
      {/if}
    </ul>
  </div>
</main>

<style>
  .todo-new {
    display: flex;
    flex-direction: row;
    justify-content: center;
    gap: 10px;
  }
  .user-input {
    padding: 10px;
    width: 60%;
  }
  h2 {
    text-decoration: underline;
  }
  ul {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  .todo-item {
    display: grid;
    grid-template-columns: 1fr 6fr 1fr 1fr;
    column-gap: 5px;
    max-width: 90%;
    margin: 0 auto;
    align-items: center;
  }
  .todo-item li {
    overflow: hidden; 
    white-space: nowrap; 
    text-overflow: ellipsis;
    text-align: left;
  }
  .todo-item button {
    justify-self: end;
  }
  .completed-true {
    color: rgb(116, 197, 100);
    text-decoration: line-through;
  }
  .completed-false {
    color: rgb(255, 209, 117);
  }
  .del-img, .add-img, .edit-img, .save-img {
    width: 15px;
    fill: #ffffff;
  }
</style>
