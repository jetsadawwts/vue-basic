<template>
<img :src="logoURL" :alt="logoCaption" width="200" height="200">
  <h1>{{ title }}</h1>
  <div>
    <span>{{ allTasks }}  {{ allTasks > 1 ? 'tasks' : 'task' }} at the moment</span>
    <br/>
    <br/>
    <input 
      type="text" 
      v-model="newTask" 
      placeholder="Add a new task" 
      @keyup.enter="addTask" />

    <button @click="addTask" :disabled="newTask.length < 1">
        AddTask
    </button>

  </div>

  <div v-if="newTask.length > 0">
    <h3>New Task preview</h3>
    <p>{{ newTask }}</p>
  </div>

  <ul>
    <!-- <li v-for="task in tasks" :key="task.id"> -->
       <!-- {{ task.id }}. {{ task.name }} -->
       <li v-for="(task, index) in latest" 
       :key="task.id"
       @click="finishTask(task)"
       :class="{ strikeout: task.finished }"
       >
          {{ index + 1 }}. {{ task.name }}

      <div v-if="task.finished">
        <button @click="removeTask(task.id)">Delete</button>
      </div>
      <div v-else-if="task.edit">
        <button>Edit</button>
      </div>
      <div v-else>
        <p>No Button</p>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      title: "My Todo App",
      tasks: [
        { id: 1, name: "Learn Vue js", finished: false },
        { id: 2, name: "Build A Vue App", finished: false },
        { id: 3, name: "Build Todo Vue", finished: false },
      ],
      newTask: "",
      logoURL: "https://images.unsplash.com/photo-1587150011803-07da1a83a4e9?ixid=MnwxMjA3fDB8MHxzZWFyY2h8Mnx8dG9kb3xlbnwwfHwwfHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60",
      logoCaption: "Learn how to build todo app with vue "
    };
  },
  methods: {
    addTask() {
      if (this.newTask.length < 1) return;

      this.tasks.push({
        id: this.tasks.length + 1,
        name: this.newTask,
        finished: false,
      });

      

      this.newTask = "";
    },
    finishTask(task) {
      task.finished = !task.finished
    },
    removeTask(taskID) {
      this.tasks = this.tasks.filter(task => {
        return task.id !== taskID;
      })
    }
  },
  computed: {
    allTasks() {
      return this.tasks.length;
    },
    latest() {
      return [...this.tasks].reverse();
    }
  } 
}
</script>

<style>

 .strikeout {
   text-decoration: line-through;
} 

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
