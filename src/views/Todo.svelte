<script>
  import { onMount, afterUpdate } from 'svelte';
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
      alertMessege('Editing', 'warning');
      todos = todos.map((item) => {
        if (item.id === taskID) {
          item.value = task;
        }
        task = '';
        isEditing = false;
        alertMessege('Task Edited!', 'success');
        return item;
      });
    }
  };

  const editItem = (id) => {
    let item = todos.find((item) => item.id === id);
    task = item.value;
    taskID = item.id;
    isEditing = true;
    alertMessege('Editing...', 'warning');
  };
  const completeItem = (id) => {
    todos = todos.map((item) => {
      if (item.id === id) {
        item.completed = !item.completed;
        console.log(item.completed);
      }
      return item;
    });
  };
  const deleteItem = (id) => {
    todos = todos.filter((task) => task.id !== id);
    alertMessege('Task Deleted!', 'danger');
  };

  const removeAll = () => {
    todos = [];
    alertMessege('No task left!', 'danger');
  };

  const setLocalStorage = () => {
    localStorage.setItem('svelteTodos', JSON.stringify(todos));
  };

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
  <div class="alert-container">
    {#if showAlert}
      <div class="alert-box {alert.type}">
        <p>{alert.messege}</p>
      </div>
    {/if}
  </div>
  <section class="main-container">
    <h2>Task List</h2>
    <form type="submit" on:submit={handleSubmit} class="form-container">
      <input
        bind:value={task}
        type="text"
        class="input-control"
        placeholder="Type something..."
        maxlength="25"
      />
    </form>
    <div class="todo-list">
      <ul class="todo-container">
        {#each todos as item}
          <li key="item.id" class="item" class:isComplete={item.completed}>
            <p class="item-value">{item.value}</p>
            <div class="btns-container">
              <button on:click={() => editItem(item.id)} class="btn edit">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M18.363 8.464l1.433 1.431-12.67 12.669-7.125 1.436 1.439-7.127 12.665-12.668 1.431 1.431-12.255 12.224-.726 3.584 3.584-.723 12.224-12.257zm-.056-8.464l-2.815 2.817 5.691 5.692 2.817-2.821-5.693-5.688zm-12.318 18.718l11.313-11.316-.705-.707-11.313 11.314.705.709z"
                  />
                </svg>
              </button>
              <button
                on:click={() => completeItem(item.id)}
                class="btn complete"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M20.285 2l-11.285 11.567-5.286-5.011-3.714 3.716 9 8.728 15-15.285z"
                  />
                </svg>
              </button>
              <button on:click={() => deleteItem(item.id)} class="btn delete">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M24 20.188l-8.315-8.209 8.2-8.282-3.697-3.697-8.212 8.318-8.31-8.203-3.666 3.666 8.321 8.24-8.206 8.313 3.666 3.666 8.237-8.318 8.285 8.203z"
                  />
                </svg>
              </button>
            </div>
          </li>
        {/each}
      </ul>
    </div>
    {#if todos.length > 0}
      <button class="remove-items" on:click={removeAll}> remove all </button>
    {/if}
  </section>
</div>
