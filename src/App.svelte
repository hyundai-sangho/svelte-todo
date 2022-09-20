<script>
  import Header from './components/Header.svelte'
  import List from './components/List.svelte'
</script>

<div class="container">
  <Header />
  <List />
</div>

<style>
  .container {
    width: 500px;
    background: #009579;
    margin: 0 auto;
  }

  :global(.container *) {
    font-family: sans-serif;
  }
</style>
