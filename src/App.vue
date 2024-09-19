<template>
  <v-app>
    <v-container></v-container>
    <v-container class="bg-overall">
      
      <v-row>
        <v-col>
          <h1 class="text-center font-weight-bold pt-3">TO DO LIST</h1>
          <h3 class="text-center pb-3">By Marc Ybiernas</h3>

          <h3>Input Task/s Here:</h3>
          <v-text-field
            v-model="newTask"
            label="Add a new task"
            @keyup.enter="addTask"
            clearable
            class="custom-text-field"
            maxlength="30"
          ></v-text-field>
          <v-btn @click="addTask" color="primary">Add Task</v-btn>
        </v-col>
      </v-row>

      <h3 class="">Added Task:</h3>
      <v-list>
        <v-list-item-group>
          <v-list-item v-for="(task, index) in tasks" :key="index">
            <v-list-item-content>
              <v-list-item-title>

                <v-text-field
                  v-model="task.text"
                  :disabled="!task.editing"
                  class="custom-text-field-list"
                  maxlength="30"
                ></v-text-field>

                <v-btn @click="toggleEdit(index)" color="green">
                  {{ task.editing ? 'Save' : 'Edit Task' }}
                </v-btn>

                <v-btn @click="deleteTask(index)" color="red">Delete</v-btn>

              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-container>
  </v-app>
</template>





<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ text: this.newTask, editing: false });
        this.newTask = '';
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleEdit(index) {
      const task = this.tasks[index];
      if (task.editing) {
        // Save the task directly when exiting edit mode
        task.editing = false;
      } else {
        // Enter edit mode
        task.editing = true;
      }
    },
  },
};
</script>




<style>


.bg-overall {
  background-color: #ECDFCC;
  border-radius: 15px;
}

.v-list-item {
  margin-bottom: 10px;
}

.custom-text-field .v-input__control {
  background-color: #1E201E;
  color: white;
  border-radius: 10px;
}

.v-list {
  padding-top: 10px;
  background-color: #3C3D37;
  border-radius: 10px;
}

.custom-text-field-list {
  background-color: white;
  color: black;
  border: 1px solid #000000; 
  font-weight: bold;
  border-radius: 10px;
  font-size: 1.2em !important;
}



.v-row {
  padding-bottom: 10px;
}

.v-btn {
  margin-top: 10px;
  margin-right: 10px;
}




</style>
