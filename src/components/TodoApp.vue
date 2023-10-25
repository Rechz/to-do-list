<template>
    <div class="container">
      <h2 class="text-center mt-5">My Todo List</h2>
    
    <!-- Input tasks -->
    <div class="d-flex">
        <input type="text" 
              placeholder="Enter task"  
              class="form-control" 
              v-model="taskInput"/>
        <button class="btn btn-warning rounded-0" 
                @click="submitData">SUBMIT</button>
    </div>

    <!-- Todo List table -->
    <table class="table table-bordered mt-5">
      <!-- Heading -->
      <thead class="text-center">
        <tr >
          <th scope="col" class="w-75 bg-info">Tasks</th>
          <th scope="col" class="w-25 bg-info">Status</th>
          <th scope="col" class="bg-info">Edit</th>
          <th scope="col" class="bg-info">Delete</th>
        </tr>
      </thead>
      <!-- Task Lists -->
      <tbody>
        <tr v-for="(task,index) in tasks" :key="index">
            <!-- Task name -->
          <th scope="row">
            <span :class="{'line-through': task.status === 'Finished'}">{{ task.name }}</span>
          </th>
          <!-- Task status -->
          <td>
            <span class="pointer noselect" 
              @click="toggleStatus(index)"
              :class="{'text-danger' : task.status === 'To-do', 
                      'text-warning' : task.status === 'In-progress', 
                      'text-success' : task.status === 'Finished' }">{{task.status}}  
            </span>       
          </td>
          <!-- Edit -->
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen pointer"></span>
            </div>
          </td>
          <!-- Delete -->
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        tasks: [],
        taskInput: '', 
        editInput : null,  
      };
    },
    methods: {
      // for Submitting data
      submitData() {
        if (this.taskInput.length === 0)
        return;
        if (this.editInput != null) {
          this.tasks[this.editInput].name = this.taskInput;
          this.editInput = null;
        }
        else {
          this.tasks.push({name: this.taskInput, status: 'To-do', click: 0})
        } 
        this.taskInput = '';
      },

        // for toggling status
      toggleStatus(index) {
        this.tasks[index].click++;
        switch(this.tasks[index].click)
        {
          case 1: this.tasks[index].status = 'In-progress';
          break;
          case 2: this.tasks[index].status = 'Finished';
          break;
          default:
          return;    
        }
      },

      // For deleting task
      deleteTask(index) {
        this.tasks.splice(index,1);
      },

      // For editing task
      editTask(index) {
        this.taskInput = this.tasks[index].name;
        this.editInput = index;
      }
    },
 }
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.line-through{
  text-decoration: line-through;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
</style>
