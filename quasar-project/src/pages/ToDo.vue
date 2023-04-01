<template>
  <q-page class="bg-grey-3 column">
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

    return {
      confirmBtn,
      onDone,
      onDelete,
      tasks,
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
