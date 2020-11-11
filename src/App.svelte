<script>
  import { setContext, onMount, afterUpdate } from 'svelte';

  // Components
  import Navbar from './Navbar.svelte';
  import ExpensesList from './ExpensesList.svelte';
  import Totals from './Totals.svelte';
  import ExpenseForm from './ExpenseForm.svelte';
  import Modal from './Modal.svelte';

  // Data
  // import expenseData from './expenses';

  // Variables
  let expenses = [];
  let isFormOpen = false;

  // Variables for Editing Expense
  let setId = null;
  let setName = '';
  let setAmount = null;

  // Reactives
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);

  // Functions
  const removeExpense = (id) => {
    expenses = expenses.filter((expense) => expense.id != id);
  };

  const clearExpenses = () => {
    expenses = [];
  };

  const addExpense = ({ name, amount }) => {
    let expense = { id: Math.random() * Date.now(), name, amount };
    expenses = [expense, ...expenses];
  };

  const setModifiedExpense = (id) => {
    let expense = expenses.find((expense) => expense.id == id);

    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  };

  const editExpense = ({ name, amount }) => {
    expenses = expenses.map((expense) => {
      return expense.id === setId
        ? { ...expense, name, amount }
        : { ...expense };
    });
    setId = null;
    setName = '';
    setAmount = null;
  };

  const showForm = () => (isFormOpen = true);

  const hideForm = () => {
    isFormOpen = false;
    setId = null;
    setName = '';
    setAmount = null;
  };

  // Context
  setContext('remove', removeExpense);
  setContext('modify', setModifiedExpense);

  // Local storage
  const setLocalStorage = () => {
    localStorage.setItem('expenses', JSON.stringify(expenses));
  };

  // Lifecycle Functions
  onMount(() => {
    expenses = localStorage.getItem('expenses')
      ? JSON.parse(localStorage.getItem('expenses'))
      : [];
  });

  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<!-- HTML -->
<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {addExpense}
        name={setName}
        amount={setAmount}
        {isEditing}
        {editExpense}
        {hideForm} />
    </Modal>
  {/if}
  <Totals title="Total Expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>Clear Expenses</button>
</main>
