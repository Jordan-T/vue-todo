<template>
  <div class="c-todoView">
    <h1 class="c-todoView__title">Todo view</h1>
    <div class="c-todoView__list">
      <Todo title="Test Todo" />
      <Todo title="Test Todo reversed" add-first />
    </div>
  </div>
</template>
<script lang="ts">
import Todo from "@/components/todo/Todo.vue";
import Vue from "vue";
import Component from "vue-class-component";

@Component({
  components: { Todo }
})
export default class TodoView extends Vue {}
</script>

<style lang="scss">
.c-todoView {
  &__list {
    $space: 30px;
    display: flex;
    flex-wrap: wrap;
    align-items: stretch;
    justify-content: center;
    width: calc(100% + #{$space});
    margin-left: $space / -2;
    margin-right: $space / -2;

    > * {
      flex-basis: calc(30% - #{$space});
      margin: $space / 2;
    }
  }
}
</style>
