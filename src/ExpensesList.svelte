<script>
  import { fly } from 'svelte/transition';
  import { flip } from 'svelte/animate';
  import { quintOut } from 'svelte/easing';
  import SectionTitle from './Title.svelte';
  import Expense from './Expense.svelte';

  export let expenses = [];
</script>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>

<!-- HTML -->
<section>
  <SectionTitle title="expense list" />
  <ul>
    {#each expenses as expense, index (expense.id)}
      <div
        in:fly={{ x: 300, delay: (index + 1) * 300 }}
        out:fly={{ x: 300, delay: (index + 1) * 100 }}
        animate:flip={{ delay: 500, easing: quintOut }}>
        <Expense {...expense} />
      </div>
    {:else}
      <h2>No expenses added to the list</h2>
    {/each}
  </ul>
</section>
