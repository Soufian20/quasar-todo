<template>
  <q-page class="bg-grey-3 column">
    <q-list class="bg-white" :separator="true" :bordered="true">
      <q-item v-ripple> </q-item>
      <q-item
        v-for="(task, index) in state.tasks"
        :key="index"
        v-ripple
        :clickable="true"
        @click="task.done = !task.done"
        :class="{ 'done bg-green-1': task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            color="primary"
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section :side="true">
          <q-btn
            :flat="true"
            :round="true"
            color="primary"
            icon="delete"
            @click.stop="deleteTask(index)"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <q-btn
      :flat="true"
      :round="true"
      color="primary"
      icon="add"
      @click.stop="addTask()"
    />
  </q-page>
</template>

<script>
import { defineComponent, reactive } from "vue";
import { useQuasar } from "quasar";

export default defineComponent({
  name: "TodoPage",

  setup() {
    const $q = useQuasar();
    const state = reactive({
      tasks: [
        {
          title: "Test1",
          done: false,
        },
        {
          title: "Test2",
          done: false,
        },
      ],
    });

    function addTask() {
      const task = { title: "", done: false };
      state.tasks.push(task);
    }

    function deleteTask(index) {
      state.tasks.splice(index, 1);
      $q.notify({
        avatar: "https://cdn.quasar.dev/img/boy-avatar.png",
        message: "Task deleted",
        color: "green",
      });
    }
    return {
      state,
      addTask,
      deleteTask,
    };
  },
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
