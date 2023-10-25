<script lang="ts">
  import { v4 as uuidv4 } from "uuid";
  import TodoList from "./TodoList.svelte";
  let inputItem: string = "";
  let count: number;
  let todos: any = JSON.parse(localStorage.getItem("todos") || "[]") || [];
  countFilter();

  function handleForm(e: any) {
    e.preventDefault();
    if (inputItem.trim() !== "") {
      let newItem = {
        id: uuidv4(),
        name: inputItem.trim(),
        completed: false,
      };
      todos = [...todos, newItem];
      inputItem = "";
      localStorage.setItem("todos", JSON.stringify(todos));
      countFilter();
    }
  }

  function countFilter() {
    count = todos.filter((todo: any) => todo.completed === false).length;
  }
</script>

<main>
  <form on:submit={handleForm}>
    <h2>Agregar Tarea</h2>
    <div class="todo-new">
      <input
        type="text"
        name="newitem"
        bind:value={inputItem}
        autocomplete="off"
        class="user-input"
        autofocus={true}
      />
      <button>
        <img
          src="./src/assets/plus-solid.svg"
          alt="Boton agregar"
          class="add-img"
        />
      </button>
    </div>
  </form>
  <TodoList {todos} {count} {countFilter} />
</main>

<style>
</style>
