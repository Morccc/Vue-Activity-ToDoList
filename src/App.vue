<template>
  <v-app>
  <v-container>
  </v-container>
    <v-container class="bg-overall">
    <v-row>
      <v-col>
        <h1 class="text-center font-weight-bold py-3">TO DO LIST</h1>
        <v-text-field
          v-model="newTask"
          label="Add a new task"
          @keyup.enter="addTask"
          clearable
          class="custom-text-field"
          ></v-text-field>
        <v-btn @click="addTask" color="primary">Add Task</v-btn>
      </v-col>
    </v-row>

    <h3 class="text-black">Added Task:</h3>
    <v-list>
      <v-list-item-group>
        <v-list-item v-for="(task, index) in tasks" :key="index">
          <v-list-item-content>
            <v-list-item-title>
              <v-text-field
                v-model="task.text"
                @blur="updateTask(index)"
                @keyup.enter="updateTask(index)"
                class="custom-text-field-list" 
                :disabled="!task.editing"
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
        // Save the task when exiting edit mode
        this.updateTask(index);
      }
      // Toggle the editing state
      task.editing = !task.editing;
    },
    updateTask(index) {
      // Optional: Implement logic to persist changes here
      // Set editing to false, will be handled in toggleEdit if saved
      this.tasks[index].editing = false;
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
}

.v-list {
  padding-top: 10px;
  background-color: #3C3D37;
}

.custom-text-field-list {
  background-color: #697565;
  color: white;
}


.v-row {
  padding-bottom: 10px;
}

.v-btn {
  margin-top: 10px;
  margin-right: 10px;
}




</style>
