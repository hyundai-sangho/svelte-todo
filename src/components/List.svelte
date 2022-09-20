<script>
  import NewItem from './NewItem.svelte'
  import { onMount } from 'svelte'
  import { items } from '../stores'
  import TodoApi from '../TodoApi'
  import Item from './Item.svelte'
  import { v4 as uuidv4 } from 'uuid'

  function 할일등록(e) {
    $items = [
      {
        id: uuidv4(),
        text: e.detail,
        completed: false,
      },
      ...$items,
    ]

    TodoApi.save($items)
  }
  function 업데이트(e) {
    const index = $items.findIndex((item) => item.id === e.detail.id)
    $items[index] = e.detail

    TodoApi.save($items)
  }
  function 삭제(e) {
    $items = $items.filter((item) => item.id !== e.detail)
    TodoApi.save($items)
  }

  let itemSorted = []

  $: {
    itemSorted = [...$items]
    itemSorted.sort((a, b) => {
      if (a.completed && b.completed) return 0
      if (a.completed) return 1
      if (b.completed) return -1
    })
  }

  onMount(async () => {
    $items = await TodoApi.getAll()
  })
</script>

<div class="list">
  <NewItem on:newitem={할일등록} />
  {#each itemSorted as item (item)}
    <Item {...item} on:update={업데이트} on:delete={삭제} />
  {:else}
    <p class="list-status">항목이 없습니다.</p>
  {/each}
</div>

<style>
  .list {
    padding: 15px;
  }

  .list-status {
    margin: 0;
    text-align: center;
    color: #ffffff;
    font-weight: bold;
    font-size: 1.1em;
  }
</style>
