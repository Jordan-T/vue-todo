<template>
  <div class="c-todoItem" :class="{ 'c-todoItem--done': todo.done }">
    <form v-if="editing" @submit.prevent="submitEdit" @keyup.esc="cancelEdit">
      <input ref="input" v-model="editText" type="text" />
      <button type="submit">Valider</button>
    </form>
    <template v-else>
      <input type="checkbox" :checked="todo.done" @input="updateDone" />
      <p class="c-todoItem__text">{{ todo.text }}</p>
      <button ref="edit" type="button" @click.prevent="edit">Edit</button>
      <button type="button" @click.prevent="deleteTodo">Delete</button>
    </template>
  </div>
</template>

<script lang="ts">
import { Todo } from "@/components/todo/Todo.vue";
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class TodoItem extends Vue {
  @Prop() private todo!: Todo;

  editing = false;
  editText = "";

  edit() {
    this.editText = this.todo.text;
    this.editing = true;
    this.$nextTick(() => {
      (this.$refs.input as HTMLInputElement).focus();
    });
  }

  submitEdit() {
    if (this.editText !== this.todo.text) {
      this.$emit("update:todo", {
        ...this.todo,
        text: this.editText
      });
    }

    this.cancelEdit();
  }

  cancelEdit() {
    this.editing = false;
    this.editText = "";
    this.$nextTick(() => {
      (this.$refs.edit as HTMLButtonElement).focus();
    });
  }

  updateDone(event: Event) {
    if (event.target === null) {
      return;
    }

    const target = event.target as HTMLInputElement;
    this.$emit("update:todo", {
      ...this.todo,
      done: target.checked
    });
  }

  deleteTodo() {
    this.$emit("delete");
  }
}
</script>

<style lang="scss">
.c-todoItem {
  display: flex;
  align-items: center;
  width: 100%;

  &__text {
    margin: 0;
    flex-grow: 1;
    text-align: left;
  }

  &--done {
    opacity: 0.5;
    text-decoration: line-through;
  }
}
</style>
