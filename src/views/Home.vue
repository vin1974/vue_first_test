<template>
  <div class="home">
    <div class="container">
        <div v-if="showAddTask">
          <AddTask @add-task="addTask" />
        </div>
        
        <Header 
          title="Task Tracker" 
          @toggle-add-task="toggleAddTask"
          v-bind:showAddTask="this.showAddTask"
          />

        <Tasks 
          @toggle-reminder="toggleReminder"
          @delete-task="deleteTask"
          v-bind:tasks="tasks" />
        
        <Footer />

    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import Header from '../components/Header'
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
import Footer from '../components/Footer'

export default {
  name: 'Home',
  components: {
    Header,
    Tasks,
    AddTask,
    Footer
  },
  data() {
    return {
      tasks: [],
      showAddTask: false
    }
  },
  methods: {
    async addTask(newTask){
      // this.tasks.push(newTask);
      // this.tasks = [...this.tasks, newTask];
    
      const res = await fetch('api/tasks', {
        method: 'post',
        headers: {
          "Content-type": 'application/json'
        },
        body: JSON.stringify(newTask)
      });

      const data = await res.json()

      this.tasks = [...this.tasks, data]
    
    },
    async deleteTask(id){
      if (confirm('Do you want to delete?')) {
        // this.tasks = 
        //   this.tasks.filter((task) => task.id !== id);
        const res = await fetch(`api/tasks/${id}`, {
          method: "delete",
          headers: {
            "Content-type": "application/json",
          }

        });

        res.status === 200 ? (this.tasks = this.tasks.filter((task)=> task.id !== id)): alert("Error deleting data!!!");

      }
    },
    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id);
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      // console.log("updated request: ", updTask);

      const res = await fetch(`api/tasks/${id}`, {
        method: 'put',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updTask)
      });

      const data = await res.json()

      res.status === 200 ? (this.tasks = this.tasks.map((task)=> task.id === id ? {...task, reminder: data.reminder} : task)): alert('Error updating data');

      // const res = await fetch(`api/tasks/${id}`, {
      //   method: 'update',
      //   header: {
      //     "Content-type": "application/json",
      //   },
      //   body: ""
      // });
      // this.tasks = this.tasks.map((task)=> task.id === id ? {...task, reminder: !task.reminder} : task);
    },
    toggleAddTask(){
      this.showAddTask = !this.showAddTask;
    },
    async fetchTasks(){
      const res = await fetch('api/tasks');

      const data = await res.json();

      return data;
    },
    async fetchTask(id){
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();

      return data;
    }

  },

  //fetch from json server
  async created(){
    // console.log('hi....eppie...')
    this.tasks = await this.fetchTasks();
  },



  //lifecycle
  // created() {
  //   this.tasks = [
  //     {
  //       id: 1,
  //       text: 'Code Vue',
  //       day: 'March 1st at 2:30 pm',
  //       reminder: true
  //     },
  //     {
  //       id: 2,
  //       text: 'go to Laundary',
  //       day: 'March 2nd at 8:00 am',
  //       reminder: false
  //     },
  //     {
  //       id: 3,
  //       text: 'Work',
  //       day: 'March 3rd at 12:30 pm',
  //       reminder: true
  //     }
  //   ]
  // }
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
    transform: scale(0.98)
  }

  .btn-block {
    display: block;
    width: 100%;
  }

</style>
