<script>
  const url = window.location.href;
  let array = url.split("/");
  const id = array[array.length - 1];
  let task;
  let updatedTitle = '';
  let updatedDescription = '';
  let completed = false;

  const findTask = async (id) => {
    const res = await fetch(`http://localhost:3000/todo/${id}/edit`, {
      method: "Get",
      headers: {},
    });
    task = await res.json();
    completed = task.foundTask.completed;
  };
  const update = async () => {
    updatedTitle = task.foundTask.task;
    updatedDescription = task.foundTask.description;
    const data = {
      task: updatedTitle,
      description: updatedDescription,
      completed: completed
    }
    const res = await fetch(`http://localhost:3000/todo/${id}`, {
      method: "PUT",
      body: JSON.stringify(data),
      headers: { "Content-Type": "application/json" },
    });
    location.replace('http://localhost:8080');
  }

</script>
  {#await findTask(id) then}
    <div>
      <form class="todo-form" on:submit={update}>
        <label for="title"> <b>Task Name</b> </label>
        <input
          bind:value={task.foundTask.task}
          type="text"
          id="title"
          class="todos__input"
          placeholder="Enter Title"
        />
        <label for="description"><b>Description</b></label>
      <input
        bind:value={task.foundTask.description}
        type="text"
        id="description"
        class="todos__input"
        placeholder="Enter Description"
      />
      <input
        bind:value={task.foundTask.completed}
        type="checkbox"
        id="completed"
        checked= {completed}
        on:click={() => {completed = !completed}}
      />
      <label for="completed">completed</label>
      <button class="bg-dark"> Update </button>
      </form>
    </div>
  {/await}

<style>
  input[type="text"] {
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    display: inline-blreflectock;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }

 button {
    width: 100%;
    color: white;
    padding: 14px 20px;
    margin: 8px 0;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  div {
    border-radius: 5px;
    margin: 50px 80px 0px 80px;
    padding: 20px;
  }
  
</style>