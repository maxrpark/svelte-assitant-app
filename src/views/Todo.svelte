<script>
  import { onMount, afterUpdate, setContext } from 'svelte';
  import Form from '../components/Form.svelte';
  import TodoItem from '../components/TodoItem.svelte';
  import Alert from '../components/Alert.svelte';
  class Todo {
    constructor(value) {
      this.value = value;
      this.id = new Date().getTime().toString();
      this.completed = false;
    }
  }
  // variables
  let todos = [];
  let task = '';
  let showAlert = false;
  let isEditing = false;
  let taskID = '';
  let alert = {
    messege: null,
    type: null,
  };

  // alert
  let timeOut;
  const alertMessege = (messege, type) => {
    clearTimeout(timeOut);
    showAlert = true;
    alert.messege = messege;
    alert.type = type;
    if (!isEditing) {
      timeOut = setTimeout(() => {
        showAlert = false;
      }, 1500);
    }
  };

  // form
  const handleSubmit = (e) => {
    e.preventDefault();
    if (task.trim() === '') {
      alertMessege('Please enter a task', 'danger');
    } else if (!isEditing) {
      const item = new Todo(task);
      task = '';
      todos.push(item);
      alertMessege('Task Added!', 'success');
      todos = todos;
    } else if (isEditing) {
      todos = todos.map((item) => {
        if (item.id === taskID) {
          item.value = task;
        }
        isEditing = false;
        alertMessege('Task Edited!', 'success');
        return item;
      });
      task = '';
    }
  };

  // edit
  const editItem = (id) => {
    let item = todos.find((item) => item.id === id);
    task = item.value;
    taskID = item.id;
    isEditing = true;
    alertMessege('Editing...', 'warning');
  };

  // complete
  const completeItem = (id) => {
    task = '';
    isEditing = false;
    todos = todos.map((item) => {
      if (item.id === id) {
        item.completed = !item.completed;
      }
      return item;
    });
  };

  // delete
  const deleteItem = (id) => {
    task = '';
    isEditing = false;
    todos = todos.filter((task) => task.id !== id);
    alertMessege('Task Deleted!', 'danger');
  };

  // clear
  const removeAll = () => {
    task = '';
    isEditing = false;
    todos = [];
    alertMessege('No task left!', 'danger');
  };

  // local storage
  const setLocalStorage = () => {
    localStorage.setItem('svelteTodos', JSON.stringify(todos));
  };

  // context

  setContext('deleteItem', deleteItem);
  setContext('editItem', editItem);
  setContext('completeItem', completeItem);
  setContext('handleSubmit', handleSubmit);

  onMount(() => {
    if (localStorage.getItem('svelteTodos')) {
      todos = JSON.parse(localStorage.getItem('svelteTodos'));
    } else {
      todos = [];
    }
  });
  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<div class="section-center section">
  <h2 class="title">Svelte assistant</h2>
  <Alert {showAlert} {alert} />
  <section class="main-container">
    <h2>Task List</h2>
    <Form bind:task />
    <div class="todo-list">
      <ul class="todo-container">
        {#each todos as item (item.id)}
          <TodoItem {item} />
        {/each}
      </ul>
    </div>
    {#if todos.length > 0}
      <button class="remove-items" on:click={removeAll}> remove all </button>
    {/if}
  </section>
</div>
