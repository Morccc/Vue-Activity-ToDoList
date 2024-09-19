<template>
  <v-app>
    <v-container class="bg-overall vh-100">
      <v-row>
        <v-col>
          <h1 class="text-center font-weight-bold pt-3">TO DO LIST</h1>
          <h3 class="text-center pb-3">By Marc Ybiernas</h3>

          <div class="text-center pb-3">
            <v-btn @click="openSchedule" color="info" small>View My Schedule</v-btn>
          </div>

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
                <div class="d-flex flex-column task-wrapper">
                  <div class="d-flex align-center mb-2 task-container">
                    <v-checkbox
                      v-model="task.completed"
                      @change="toggleCompletion(index)"
                      color="green"
                      class="checkbox-custom"
                    ></v-checkbox>
                    <v-text-field
                      v-model="task.text"
                      :disabled="!task.editing"
                      class="custom-text-field-list"
                      maxlength="30"
                      style="flex: 1; margin-left: 10px;"
                    ></v-text-field>
                  </div>
                  
                  <div class="text-center">
                    <v-btn @click="toggleEdit(index)" color="green" class="mr-2">
                      {{ task.editing ? 'Save' : 'Edit Task' }}
                    </v-btn>
                    <v-btn @click="deleteTask(index)" color="red">Delete</v-btn>
                  </div>

                  <div class="text-white mt-2">
                    <small>Added: {{ task.addedTime }}</small><br />
                    <small v-if="task.updatedTime">Updated: {{ task.updatedTime }}</small><br />
                    <small v-if="task.completedTime">Completed: {{ task.completedTime }}</small>
                  </div>
                </div>
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>

      <v-dialog v-model="scheduleDialog" max-width="2000px">
        <v-card class="text-center">
          <v-card-title class="text-h5 text-center">My Current Schedule</v-card-title>
          <v-card-text style="overflow-y: auto; max-height: 700px;">
            <v-img :src="scheduleImage" aspect-ratio="1.7" contain></v-img>
          </v-card-text>
          <v-card-actions class="justify-center">
            <v-btn @click="scheduleDialog = false" color="primary">Close</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-container>
  </v-app>
</template>

<script>
import scheduleImage from '@/assets/schedule.png'; 

export default {
  data() {
    return {
      newTask: '',
      tasks: [],
      scheduleDialog: false,
      scheduleImage: scheduleImage, 
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        const currentTime = new Date().toLocaleString();
        this.tasks.push({ 
          text: this.newTask, 
          editing: false, 
          addedTime: currentTime, 
          updatedTime: null,
          completed: false,
          completedTime: null,
        });
        this.newTask = '';
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleEdit(index) {
      const task = this.tasks[index];
      if (task.editing) {
        task.updatedTime = new Date().toLocaleString();
        task.editing = false;
      } else {
        task.editing = true;
      }
    },
    toggleCompletion(index) {
      const task = this.tasks[index];
      if (task.completed) {
        task.completedTime = new Date().toLocaleString();
      } else {
        task.completedTime = null;
      }
    },
    openSchedule() {
      this.scheduleDialog = true;
    },
  },
};
</script>

<style>
.bg-overall {
  background-color: transparent;
  border: 5px solid black;
  border-radius: 15px;
  min-height: 100vh;
  height: auto; 
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
  margin-top: 10px;
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
  flex: 1;
}

.v-row {
  padding-bottom: 10px;
}

.v-btn {
  margin-top: 10px;
  margin-right: 10px;
}

.checkbox-custom {
  display: flex; 
  border: 2px solid white;
  background-color: white; 
  border-radius: 10%; 
}

.task-wrapper {
  overflow-x: auto; /* Allow horizontal scrolling */
  white-space: nowrap; /* Prevent line breaks */
}


.task-container {
  display: flex;
  align-items: center;
  min-width: 100%; /* Ensures it takes up enough space to trigger scrolling */
}

</style>
