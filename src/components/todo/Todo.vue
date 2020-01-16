<template>
  <div class="c-todo">
    <p class="c-todo__title">{{ title }}</p>
    <TodoAdd @add="onAdd" />
    <TodoItem
      v-for="(todo, i) in todos"
      :key="todo.id || i"
      :todo.sync="todos[i]"
      @delete="deleteTodo(todo, i)"
    />
    <div v-if="hasDone" class="c-todo__footer">
      <button type="button" @click.prevent="deleteDone">
        Delete done
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import TodoAdd, { TodoAdded } from "@/components/todo/TodoAdd.vue";
import TodoItem from "@/components/todo/TodoItem.vue";
import { Component, Prop, Vue } from "vue-property-decorator";

export interface Todo {
  id: number;
  text: string;
  done: boolean;
}

@Component({
  components: { TodoAdd, TodoItem }
})
export default class Todos extends Vue {
  @Prop() private title!: string;
  @Prop({ type: Boolean, default: false }) private addFirst!: boolean;

  todos: Todo[] = [];
  previousId = 0;

  get hasDone() {
    return this.todos.filter(todo => todo.done).length !== 0;
  }

  getNewId(): number {
    this.previousId += 1;
    return this.previousId;
  }

  onAdd(added: TodoAdded) {
    const newTodo: Todo = {
      id: this.getNewId(),
      text: added.text,
      done: false
    };

    if (this.addFirst) {
      this.todos.unshift(newTodo);
    } else {
      this.todos.push(newTodo);
    }

    added.resetTodo();
  }

  deleteTodo(todo: Todo, index?: number) {
    if (index === undefined) {
      index = this.todos.indexOf(todo);
    }

    this.todos.splice(index, 1);
  }

  deleteDone() {
    this.todos = this.todos.filter(todo => !todo.done);
  }
}
</script>

<style lang="scss">
.c-todo {
  padding: 10px;
  border: solid 1px #ccc;
  border-radius: 4px;

  &__title {
    padding: 10px;
    margin: -10px -10px 10px;
    border-bottom: solid 1px #ccc;
    font-weight: 700;
  }

  &__footer {
    padding: 10px;
    margin: 10px -10px -10px;
    border-top: solid 1px #ccc;
  }
}
</style>
