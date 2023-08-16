<script>
  import { link } from "svelte-spa-router";
  import date from 'date-and-time';
  import Icon from "./Icon.svelte";

  export let item;
  export let completed;

  const updateItem = async (id) => {
    let data = {
      completed: !completed,
    };
    completed = !completed;
    const res = await fetch(`http://localhost:3000/todo/${id}`, {
      method: "PUT",
      body: JSON.stringify(data),
      headers: { "Content-Type": "application/json" },
    });
    const output = res.json();
    if (res.ok) {
      location.reload();
    }
  };
  const removeFromList = async (id) => {
    const res = await fetch(`http://localhost:3000/todo/${id}`, {
      method: "DELETE",
      headers: {},
    });
    const output = res.json();
    window.location.reload();
  };

</script>

<div class="container">
  <div class="todo">
    <a href="/task/{item._id}" use:link>
      <span class="todo__text">{item.task}</span> <br>
      <span>{date.format(new Date(item.updatedAt),'ddd, MMM DD YYYY HH:mm')} </span>
    </a>
    <div class="icons">
      <button class="icon__button" on:click={updateItem(item._id)}>
        <Icon name="check-mark" class="icon" />
      </button>
      <button class="icon__button" on:click={removeFromList(item._id)}>
        <Icon name="delete" class="icon" />
      </button>
      <span class={item.completed ? "checked--strike" : "display"}>
        <Icon name="done-mark" class="icon" />
      </span>
    </div>
  </div>
</div>

  <style>
    a:link {
    text-decoration: none;
    }
  .display {
    display: none;
  }
  .checked--strike {
    color: green;
  }
  .icon__button {
    background-color: transparent;
    border: none;
    box-shadow: none;
    font-size: 1.2rem;
    cursor: pointer;
    color: rgba(0, 0, 0, 0.54);
  }
  .todo {
    display: flex;
    padding: 20px;
    border-radius: 20px;
    box-shadow: 0 0 15px rgb(0 0 0 / 20%);
    background-color: hsla(0, 0%, 100%, 0.2);
    -webkit-backdrop-filter: blur(25px);
    backdrop-filter: blur(25px);
    width: inherit;
    margin-top: 15px;
    font-size: 1.2rem;
    justify-content: space-between;
    align-items: center;
  }
  </style>