<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        v-model="taskItem"
        @keyup.enter="onAdd(taskItem)"
        square
        placeholder="Add task"
        bg-color="white"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="onAdd(taskItem)" />
        </template>
      </q-input>
    </div>

    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        v-ripple
        @click="onDone(task)"
        :class="task.done === true ? 'done bg-blue-1' : null"
        clickable
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

        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="onDelete(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import { useQuasar } from "quasar";
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "ToDo",

  setup() {
    const $q = useQuasar();
    const tasks = ref([
      { title: "Get bananas", done: false },
      { title: "Eat bananas", done: false },
      { title: "Poo bananas", done: false },
    ]);
    const confirmBtn = ref(false);
    const taskItem = ref("");

    const onDone = (value) => {
      value.done = !value.done;
    };

    const onDelete = (value) => {
      $q.dialog({
        title: "Confirm",
        message: "Are you sure you want to delete?",
        cancel: false,
        persistent: true,
      }).onOk(() => {
        tasks.value.splice(value, 1);
        $q.notify("Task deleted");
      });
    };

    const onAdd = (value) => {
      tasks.value.push({ title: value, done: false });
    };

    return {
      onDone,
      onDelete,
      onAdd,
      confirmBtn,
      tasks,
      taskItem,
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
