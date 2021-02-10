<template>
  <div>
    <section class="todoapp">
      <header class="header">
        <h1>{{ title }}</h1>
        <input
            class="new-todo"
            placeholder="What needs to be done?"
            autofocus
        />
      </header>

    </section>
  </div>
</template>

<script>

export default {
  name: "todo",
  data() {
    return {
      title: 'Todo-List',
    };
  },
};
</script>