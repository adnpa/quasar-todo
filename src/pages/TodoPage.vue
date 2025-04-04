<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        class="col"
        square
        filled
        v-model="newTask"
        placeholder="Add Task"
        dense
        bg-color="white"
      >
        <template v-slot:append>
          <q-btn
            @click="addTask"
            round
            dense
            flat
            icon="add"
          />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white">
      <q-item
        v-for="task, idx in tasks"
        :key="idx"
        @click="task.done = !task.done"
        v-ripple
        clickable
        :class="{ 'done bg-blue-1': task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            class="no-pointer-events"
            v-model="task.done"
            color="primary"
          />
        </q-item-section>

        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <q-item-section
          v-if="task.done"
          side
        >
          <q-btn
            @click.stop="deleteTask(idx)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>

      </q-item>
    </q-list>

    <div
      v-if="!tasks.length"
      class="no-tasks absolute-center"
    >
      <q-icon
        name="check"
        size="100px"
        color="primatry"
      />

      <div class="text-h5 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar';
import { reactive, ref } from 'vue';

const $q = useQuasar()

const newTask = ref("")

const tasks = reactive([
  { title: "Get abc", done: false },
  { title: "bbb abc", done: false },
  { title: "ccc abc", done: false },
])

function addTask() {
  tasks.push({
    title: newTask.value,
    done: false,
  })
  newTask.value = ""
}

function deleteTask(idx) {
  $q.dialog({
    title: 'Confirm',
    message: 'Really Delete',
    cancel: true,
    persistent: true,
  }).onOk(() => {
    console.log(idx)
    this.tasks.splice(idx, 1)
    $q.notify("Task Deleted")
  })
}

</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.5;
}
</style>