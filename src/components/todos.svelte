<script>
  export let todos = [];
  let newTodo = "";

  $: newTodoId = totalTodos ? Math.max(...todos.map((t) => t.id)) + 1 : 1;
  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;

  function removeTodo(id) {
    todos = todos.filter((t) => t.id !== id);
  }

  function addNewTodo() {
    todos = [
      ...todos,
      {
        id: newTodoId,
        name: newTodo,
        completed: false,
      },
    ];
    newTodo = "";
  }

  let filter = "all";
  const filterTodos = (filter, todos) =>
    filter === "active"
      ? todos.filter((t) => !t.completed)
      : filter === "completed"
      ? todos.filter((t) => t.completed)
      : todos;
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">
  <!-- NewTodo -->
  <form on:submit|preventDefault={addNewTodo}>
    <h2 class="label-wrapper">
      <label for="todo-0" class="label__lg"> What needs to be done? </label>
    </h2>
    <input
      bind:value={newTodo}
      type="text"
      id="todo-0"
      autocomplete="off"
      class="input input__lg"
    />
    <button type="submit" disabled="" class="btn btn__primary btn__lg">
      Add
    </button>
  </form>

  <!-- Filter -->
  <div class="filters btn-group stack-exception">
    <button
      class="btn toggle-btn"
      class:btn__primary={filter === "all"}
      aria-pressed={filter === "all"}
      on:click={() => (filter = "all")}
    >
      <span class="visually-hidden">Show</span>
      <span>All</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button
      class="btn toggle-btn"
      class:btn__primary={filter === "active"}
      aria-pressed={filter === "active"}
      on:click={() => (filter = "active")}
    >
      <span class="visually-hidden">Show</span>
      <span>Active</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button
      class="btn toggle-btn"
      class:btn__primary={filter === "completed"}
      aria-pressed={filter === "completed"}
      on:click={() => (filter = "completed")}
    >
      <span class="visually-hidden">Show</span>
      <span>Completed</span>
      <span class="visually-hidden">tasks</span>
    </button>
  </div>

  <!-- TodosStatus -->
  <h2 id="list-heading">
    {totalTodos} out of {completedTodos} items completed
  </h2>

  <!-- Todos -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as { completed, name, id }, id}
      <li class="todo">
        <div class="c-cb">
          <input
            type="checkbox"
            id="todo-{id}"
            on:click={() => {
              completed = !completed;
            }}
            checked={completed}
          />
          <label for="todo-{id}" class="todo-label">
            {name}
          </label>
        </div>
        <div class="btn-group">
          <button type="button" class="btn">
            Edit <span class="visually-hidden">{name}</span>
          </button>
          <button
            on:click={() => removeTodo(id)}
            type="button"
            class="btn btn__danger"
          >
            Delete <span class="visually-hidden">{name}</span>
          </button>
        </div>
      </li>
    {:else}
      Nothing to do here!
    {/each}
  </ul>

  <hr />

  <!-- MoreActions -->
  <div class="btn-group">
    <button type="button" class="btn btn__primary">Check all</button>
    <button type="button" class="btn btn__primary">Remove completed</button>
  </div>
</div>
