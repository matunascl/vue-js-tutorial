<template>
  <div class="container">
    <Header 
    @toggle-add-task="toggleAddTask" 
    title='Task Tracker' 
    v-bind:showAddTask="showAddTask" />
    <AddTask 
    v-if="showAddTask" 
    @add-task="addTask"/>
    <Tasks 
    @toggle-reminder="toggleReminder" 
    @delete-task="deleteTask" 
    v-bind:tasks="tasks" />
    <Footer></Footer>
  </div>
  
</template>

<script>
import Header from '@/components/Header'
import Footer from '@/components/Footer'
import Tasks from '@/components/Tasks'
import AddTask from '@/components/AddTask'

export default {
  name: 'App',
  components: {
    Header,
    Footer,
    Tasks,
    AddTask,
    
  },
  data() {
    return {
      tasks: [],
      showAddTask: false
    }
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask
    },
    async addTask(task) {
      const response = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
          },
          body: JSON.stringify(task),
        })
      const data = await response.json()
      this.tasks = [...this.tasks, data]
    },
    async deleteTask(id) {
      if(confirm('Are you sure?')) {
        const result = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        })
      result.status === 200 ? 
      (this.tasks = this.tasks.filter((task) => task.id !== id)) : 
      alert('Error deleting task')
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id)
      const updatedTask = {...taskToToggle, reminder: !taskToToggle.reminder}
      const result = await fetch(`api/tasks/${id}`, {
          method: 'PUT',
          headers: {
            'Content-type': 'application/json',
          },
          body: JSON.stringify(updatedTask),
        })
      const data = await result.json()
      this.tasks = this.tasks.map((task) => 
      task.id === id ? {...task, reminder: data.reminder } : task)
    },
    async fetchTasks() {
      const result = await fetch('api/tasks')
      const data = await result.json()
      return data
    },
    async fetchTask(id) {
      const result = await fetch(`api/tasks/${id}`)
      const data = await result.json()
      return data
    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
    }
  }

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
