<script>
  import List from "./List.svelte";

  let task = "";
  let description = "";
  let completed = false;
  let completedTask = [];
  let isCompleted = false;
  let todolist = [];
  let recaptchaResponse = null;
  const recaptcha_secret = "6Le5R8snAAAAAIR6XYnUftRB4bPcAPjJOcC3ynhe";
  $: filter = isCompleted ? completedTask : todolist;

  const loadRecaptcha = async () => {
    const script = document.createElement('script');
    script.src = `https://www.google.com/recaptcha/api.js?render=${recaptcha_secret}`;
    script.async = true;
    script.defer = true;
    document.head.appendChild(script);

    script.onload = () => {
      grecaptcha.ready(() => {
        grecaptcha.execute(recaptcha_secret, {action: 'submit'}.then(response => {
          document.getElementById('g-recaptcha-response').value = response;
        }));
      });
    };
  }

  const save = async () => {
    if (!task) {
      alert("Please fill the field");
    } else {
      const data = {
        task: task,
        description: description,
        completed: completed,
      };
      const res = await fetch("http://localhost:3000/todo", {
        method: "POST",
        body: JSON.stringify(data),
        headers: { "Content-Type": "application/json" },
      });
      const output = await res.json();
      if (output.success) {
        item = "";
        completed = false;
      }
    }
  };

  const getItems = async () => {
    loadRecaptcha();
    const res = await fetch("http://localhost:3000/todo", {
      method: "GET",
      headers: { "Content-Type": "application/json" },
    });
    todolist = await res.json();
  };

  const searchItems = async () => {
    loadRecaptcha();
    const res = await fetch("http://localhost:3000/todo/search", {
      method: "GET",
      headers: { "Content-Type": "application/json" },
    });
    completedTask = await res.json();
    return completedTask;
  };
</script>

<div class="container">
  <form class="todo-form" on:submit={save}>
    <input bind:value={task} placeholder="Enter Title" type="task" />
    <input
      bind:value={description}
      placeholder="Enter Description"
      type="task"
    />
    <button class="todos__button">ADD</button>
    <input
      type="hidden"
      id="g-recaptcha-response"
      name="g-recaptcha-response"
    />
    <input 
      type="hidden" 
      name="action" 
      value="validate_captcha" 
    />
  </form>
  <div class="row">
    <label for={isCompleted}>Show completed items</label>
    <input
      type="checkbox"
      bind:value={isCompleted}
      on:click={() => {
        isCompleted = !isCompleted;
      }}
      class="checkbox"
    />
  </div>
</div>

{#await isCompleted ? searchItems() : getItems() then}
  {#each isCompleted ? filter.completedItems : filter.itemData as item}
    <List completed={item.completed} {item} />
  {/each}
{/await}

<style>
  input[type="task"] {
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    background-color: inherit;
    border: none;
    box-shadow: none;
    display: inline-block;
    text-decoration: none;
    font-size: 1.2rem;
    border-bottom: 1px solid black;
    margin-top: 15px;
  }
  .todo-form {
    text-align: center;
    margin: 15px 0px 25px 0px;
  }
  .checkbox {
    margin: 0px 0px 10px 10px;
  }

  .todos__button {
    background-color: grey;
    width: 20%;
    color: black;
    padding: 14px 20px;
    margin: 8px 0;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
</style>
