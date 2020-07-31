<script>
	import Todos from './Todos.svelte';
</script>

<div class="page">
  <header>
    <h1>Todo</h1>
  </header>

  <main>
    <Todos />
  </main>

  <footer>
    <p>Sample project made with ❤️ by <a href="https://erikwritescode.com">Erik Kralj</a> using <a href="https://svelte.dev/">Svelte</a> and generously hosted by <a href="https://firebase.google.com/">Firebase</a>.</p>
  </footer>
</div>

<style>
  .page {
    max-width: 360px;
    margin: 0 auto;
    padding: 5px;
  }

  h1 {
    font-weight: lighter;
    font-size: 42px;
    color: orange;
    text-align: center;
  }

  @media only screen and (min-width: 420px) {
    .page {
      padding: 30px;
      margin: 30px auto;
      border: 1px solid #A3A3A3;
    }

    h1 {
      margin-top: 0;
    }
  }
</style>