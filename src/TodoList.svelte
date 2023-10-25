<script lang="ts">
  export let todos: any;
  export let count_value: number;
  export let countFilter: any;
  let editableTodoID: string = "";
  let editInput: string = "";

  function invCountFilter() {
    countFilter();
  }

  function toggleSelection(id: string) {
    todos = todos.map((todo: any) => {
      if (todo.id === id) {
        todo.completed = !todo.completed;
      }
      return todo;
    });
    localStorage.setItem("todos", JSON.stringify(todos));
    invCountFilter();
  }

  function handleDelete(id: string) {
    let todosFiltered = todos.filter((todo: any) => {
      return todo.id !== id;
    });
    todos = todosFiltered;
    localStorage.setItem("todos", JSON.stringify(todos));
    invCountFilter();
  }

  function handleEdit(id: string) {
    editableTodoID = id;
    todos.filter((todo: any) => {
      if (todo.id === id) {
        editInput = todo.name;
      }
    });
  }

  function updateTodo(id: string) {
    todos = todos.map((todo: any) => {
      if (todo.id === id) {
        todo.name = editInput;
      }
      return todo;
    });
    localStorage.setItem("todos", JSON.stringify(todos));
    invCountFilter();
    editInput = "";
    editableTodoID = "";
  }
</script>

<div class="todo-list">
  <h2>Tareas</h2>
  <ul>
    {#if todos.length > 0}
      {#if count_value === 0}
        <div class="todo-item info">
          <li>No hay nada para hacer!</li>
        </div>
      {/if}
      {#each todos as todo}
        {#if !todo.completed}
          <div class="todo-item">
            {#if editableTodoID === todo.id}
              <input
                type="checkbox"
                name="todo-{todo.id}"
                bind:checked={todo.completed}
                on:click={() => toggleSelection(todo.id)}
              />
              <input
                type="text"
                name="newitem"
                bind:value={editInput}
                autocomplete="off"
                class="user-input"
                autofocus={true}
                on:blur={() => updateTodo(todo.id)}
              />
              <button on:click={() => updateTodo(todo.id)}>
                <img
                  src="./src/assets/floppy-disk-solid.svg"
                  alt="Boton guardar"
                  class="save-img"
                />
              </button>
            {:else}
              <input
                type="checkbox"
                name="todo-{todo.id}"
                bind:checked={todo.completed}
                on:click={() => toggleSelection(todo.id)}
              />
              <li id={todo.id} class="completed-{todo.completed}">
                {todo.name}
              </li>
              <button on:click={() => handleEdit(todo.id)}>
                <img
                  src="./src/assets/pencil-solid.svg"
                  alt="Boton editar"
                  class="edit-img"
                />
              </button>
              <button on:click={() => handleDelete(todo.id)}>
                <img
                  src="./src/assets/trash-solid.svg"
                  alt="Boton borrar"
                  class="del-img"
                />
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
              <input
                type="checkbox"
                name="todo-{todo.id}"
                bind:checked={todo.completed}
                on:click={() => toggleSelection(todo.id)}
              />
              <input
                type="text"
                name="newitem"
                bind:value={editInput}
                autocomplete="off"
                class="user-input"
                autofocus={true}
                on:blur={() => updateTodo(todo.id)}
              />
              <button on:click={() => updateTodo(todo.id)}>
                <img
                  src="./src/assets/floppy-disk-solid.svg"
                  alt="Boton guardar"
                  class="save-img"
                />
              </button>
            {:else}
              <input
                type="checkbox"
                name="todo-{todo.id}"
                bind:checked={todo.completed}
                on:click={() => toggleSelection(todo.id)}
              />
              <li id={todo.id} class="completed-{todo.completed}">
                {todo.name}
              </li>
              <button on:click={() => handleEdit(todo.id)}>
                <img
                  src="./src/assets/pencil-solid.svg"
                  alt="Boton editar"
                  class="edit-img"
                />
              </button>
              <button on:click={() => handleDelete(todo.id)}>
                <img
                  src="./src/assets/trash-solid.svg"
                  alt="Boton borrar"
                  class="del-img"
                />
              </button>
            {/if}
          </div>
        {/if}
      {/each}
    {/if}
  </ul>
</div>

<style>
  @import "./styles.css";
</style>
