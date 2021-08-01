<template>
  <q-page class="q-pa-lg">
    <section class="shadow-2 q-pa-md">
      <q-input @keyup.enter="addTask" outlined v-model="newTask" label="Task">
        <template v-slot:append>
          <q-btn @click="addTask" icon="add" round dense flat />
        </template>
      </q-input>
      <div class="q-mt-md q-mb-md">
        <q-btn @click="fetchTasks" icon="sync" dense flat>
          Sync/API fetch
        </q-btn>
        <q-btn class="q-ml-md" @click="postData" icon="check" dense flat>
          Update
        </q-btn>
      </div>
      <div>
        <q-list>
          <q-item
            tag="label"
            v-for="(task, index) in tasks"
            :key="task.title"
            :class="{ task_complete: task.completed }"
            v-ripple
          >
            <q-item-section avatar>
              <q-checkbox v-model="task.completed" val="task.title" />
            </q-item-section>
            <q-item-section>
              <q-item-label>{{ task.title }}</q-item-label>
            </q-item-section>
            <!-- <q-item-section> -->
            <q-btn @click.stop="deleteTask(index)" icon="delete" dense flat />
            <!-- </q-item-section> -->
          </q-item>
        </q-list>
      </div>
    </section>
  </q-page>
</template>

<script>
import { defineComponent, Dialog } from "vue";
import { useQuasar } from "quasar";
import axios from "axios";

export default defineComponent({
  name: "PageIndex",
  data() {
    return {
      newTask: "",
      tasks: [],
    };
  },
  methods: {
    addTask() {
      if (this.newTask !== "") {
        this.tasks.push({
          title: this.newTask,
          complete: false,
        });
        this.newTask = "";
      }
    },
    fetchTasks() {
      this.$axios(`https://jsonplaceholder.typicode.com/todos`).then(response => {
        let currentTasks = this.tasks;
        this.tasks = [...currentTasks, ...response.data]
        // this.tasks = response.data;
        // console.log("response", this.tasks)
      })
    },
    postData() {
      console.log("posts",this.tasks)
        this.$axios.post(`https://jsonplaceholder.typicode.com/posts`, {
          data: this.tasks
      })
      .then(response => {
        // this.tasks = response.data;
        console.log("response", response)
      })
    },
    
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Delete this entry?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
        });
    },
  },
});
</script>

<style lang="scss">
.task_complete {
  background-color: #eee;
  .q-item__label {
    text-decoration: line-through;
  }
}
</style>
