<script>
  import { onMount } from 'svelte';
  
  let todos = [];
  let text = '';

  onMount(() => {
    load().then(_todos => todos = _todos);
  });

  function load() {
    return new Promise((resolve, reject) => {
      try {
        const todosString = window.localStorage.getItem('todos');
        const todosArray = JSON.parse(todosString);
        const todos = todosArray || [];
        resolve(todos);
      } catch (error) {
        reject(error);
      }
    });
  }

  function save(todos) {
    return new Promise((resolve, reject) => {
      try {
        todos = todos.slice();
        todos.sort((a, b) => a - b);
        window.localStorage.setItem('todos', JSON.stringify(todos));
        resolve();
      } catch (error) {
        reject(error);
      }
    });
  }

  function createTodo(event) {
    if (typeof text !== 'string') return;
    if (text.length < 1) return;

    const todo = {
      text,
      complete: false
    };

    text = '';
    todos = [...todos, todo];
    save(todos).then(() => load());
  }

  function checkChanged(event) {
    save(todos).then(() => load());
  }

  function handleRemove(event) {
    const index = parseInt(event.target.attributes['data-index'].value, 10);
    todos.splice(index, 1);
    save(todos).then(() => {
      load().then(_todos => todos = _todos);
    });
  }
</script>

<div>
	<div class="add">
    <input type="text" name="q" bind:value={text} placeholder="What's on your mind..." />
    <button on:click={createTodo}>+</button>
  </div>
  <div class="list">
    {#each todos as todo, i}
      <div class="card">
        <input type="checkbox" bind:checked={todo.checked} on:change={checkChanged} />
        <span class="text">{todo.text}</span>
        <span class="remove" on:click={handleRemove} data-index={i}>x</span>
      </div>
    {/each}
  </div>
</div>

<style>
  .add {
    display: flex;
  }

  .add input {
    flex: 1;
    margin-right: 15px;
    width: 1px;
  }

  .add button {
    flex: 0;
    box-shadow:inset 0px 1px 0px 0px #cf866c;
    background:linear-gradient(to bottom, #d0451b 5%, #bc3315 100%);
    background-color:#d0451b;
    border-radius:3px;
    border:1px solid #942911;
    display:inline-block;
    cursor:pointer;
    color:#ffffff;
    font-family:Arial;
    font-size:13px;
    padding:6px 24px;
    text-decoration:none;
    text-shadow:0px 1px 0px #854629;
  }

  .add button:hover {
    background:linear-gradient(to bottom, #bc3315 5%, #d0451b 100%);
    background-color:#bc3315;
  }

  .add button:active {
    position:relative;
    top:1px;
  }

  .list .card {
    padding: 5px;
    border: 1px solid #D7D7D7;
    display: flex;
    align-items: center;
    margin-bottom: 5px;
  }

  .list .card input {
    margin: 0 5px 0 0;
  }

  .list .card .text {
    flex: 1;
  }

  .list .card .remove {
    flex: 0 0 auto;
    padding: 5px 15px;
  }
</style>