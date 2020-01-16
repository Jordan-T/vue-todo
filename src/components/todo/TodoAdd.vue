<template>
  <form class="c-todoAdd" @submit.prevent="addTodo">
    <input v-model="text" class="c-todoAdd__input" type="text" />
    <button class="c-todoAdd__submit" type="submit">Add</button>
  </form>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

interface ResetTodo {
  (newValue?: string): void;
}
interface ResetTodoImpl {
  resetTodo: ResetTodo;
}

export interface TodoAdded {
  text: string;
  resetTodo: ResetTodo;
}

@Component
export default class TodoAdd extends Vue implements ResetTodoImpl {
  text = "";

  addTodo() {
    const addedTodo: TodoAdded = {
      text: this.text,
      resetTodo: this.resetTodo
    };
    this.$emit("add", addedTodo);
  }

  public resetTodo(newValue = "") {
    this.text = newValue;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.c-todoAdd {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
