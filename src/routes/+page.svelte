<script lang="ts">
  import { Button } from "$lib/components/ui/button";
  import { Input } from "$lib/components/ui/input";
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu";
  import * as Card from "$lib/components/ui/card";

  let todos: Todo[] = [];
  let input: string = "";
  let importance: string = "";

  function submitTodo() {
    let importanceEnum: Importance;
    switch (importance) {
      case "Important":
        importanceEnum = Importance.IMPORTANT;
        break;
      case "Semi Important":
        importanceEnum = Importance.SEMI_IMPORTANT;
        break;
      case "Not Important":
        importanceEnum = Importance.NOT_IMPORTANT;
        break;
      default:
        importanceEnum = Importance.IMPORTANT;
        break;
    }
    todos = [...todos, new Todo(input, false, importanceEnum)];
    input = "";
  }

  enum Importance {
    IMPORTANT,
    SEMI_IMPORTANT,
    NOT_IMPORTANT,
  }

  class Todo {
    name: string;
    isEditing: boolean;
    importance: Importance;
    constructor(name: string, isEditing: boolean, importance: Importance) {
      this.name = name;
      this.isEditing = isEditing;
      this.importance = importance;
    }
    setEditing() {
      this.isEditing = true;
    }
  }
</script>

<form class="flex w-full max-w-sm items-center space-x-2 mx-5 mt-5 mb-5">
  <Input type="input" placeholder="Todo" bind:value={input} />
  <DropdownMenu.Root>
    <DropdownMenu.Trigger asChild let:builder>
      <Button variant="outline" builders={[builder]}>Importance</Button>
    </DropdownMenu.Trigger>
    <DropdownMenu.Content class="w-56">
      <DropdownMenu.RadioGroup bind:value={importance}>
        <DropdownMenu.RadioItem value="Important"
          >Important</DropdownMenu.RadioItem
        >
        <DropdownMenu.RadioItem value="Semi Important"
          >Semi Important</DropdownMenu.RadioItem
        >
        <DropdownMenu.RadioItem value="Not Important"
          >Not Important</DropdownMenu.RadioItem
        >
      </DropdownMenu.RadioGroup>
    </DropdownMenu.Content>
  </DropdownMenu.Root>
  <Button type="submit" on:click={submitTodo}>Add</Button>
</form>
{#each todos as todo, i}
  <Card.Root class="w-[380px] mx-5 mt-5">
    <Card.Header>
      <div class="flex justify-between">
        {#if todo.isEditing}
          <Input type="input" bind:value={todo.name} />
        {:else}
          <Card.Title>{todo.name}</Card.Title>
        {/if}
        {#if todo.importance == Importance.IMPORTANT}
          <b class="bg-green-600 px-1 py-1 mr-0 rounded float-right"
            >Important</b
          >
        {:else if todo.importance == Importance.SEMI_IMPORTANT}
          <b class="bg-yellow-300 px-1 py-1 mr-0 rounded float-right"
            >Semi Important</b
          >
        {:else}
          <b class="bg-red-500 px-1 py-1 mr-0 rounded float-right"
            >Not Important</b
          >
        {/if}
      </div>
    </Card.Header>
    <Card.Content>
      <Button
        on:click={() => {
          todos.splice(i, 1);
          todos = todos;
        }}>Delete</Button
      >
      {#if todo.isEditing}
        <Button
          on:click={() => {
            todo.isEditing = false;
          }}>Save</Button
        >
      {:else}
        <Button
          on:click={() => {
            todo.isEditing = true;
          }}
          >Edit
        </Button>
      {/if}
    </Card.Content>
  </Card.Root>
{/each}
