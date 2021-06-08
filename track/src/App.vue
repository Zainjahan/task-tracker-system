<template>
  <div class="container">
    <CustomHeader @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask"/>
  <div v-show="showAddTask">
    <AddTask @add-task="addTask" />
    
  </div>
  
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      v-if="tasks"
      :tasks="tasks"
    />
    <router-view></router-view>
    <Footer/>
    
  </div>
</template>

<script>
import CustomHeader from "./components/CustomHeader";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";
import Footer from "./components/Footer";




export default {
  name: "App",
  components: {
    CustomHeader,
    Footer,
    Tasks,
    AddTask,
    
    
  },
  data() {
    return {
      tasks: [],
      showAddTask:true
    };
  },
  methods: {
    toggleAddTask(){
this.showAddTask=!this.showAddTask
    },
   async  addTask(task) {
  //   const res=await fetch('api/tasks',{
      // method :'POST',
     //  headers:{
      //  'Content-type':'application/json',
       //  body:JSON.stringify(task),
      // }
      
    // })
     //const data =await res.json()

      this.tasks = [...this.tasks, task];
    },
    deleteTask(id) {
      if (confirm("Are you sure")) {
        // console.log('task',id)
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    toggleReminder(id) {
      // console.log(id, 'inapp')
      let toggleTaskId = this.tasks.findIndex((task) => task.id === id);
      if (toggleTaskId > -1) {
        this.tasks[toggleTaskId].reminder = !this.tasks[toggleTaskId].reminder;
      }
      // this.tasks.forEach((task) => {
      //   if (task.id === id) task.reminder = !task.reminder;
      // task.reminder = task.id === id ? !task.reminder : task.reminder;
      // task.id === id ? { ...task, reminder: !task.reminder } : task;
      // });
      // this.task = this.tasks.map((task) =>
      //   task.id === id ? { ...task, reminder: !task.reminder } : task
      // );
      
    },
    async fetchTasks(){
      const res=await fetch('api/tasks')

      const data=await res.json()
      return data
    },
  //  async fetchTasks(id){
    //  const res=await fetch(`api/tasks/${id}`)

    //  const data=await res.json()
     // return data
   // }
  },

  created() {
    this.tasks = [
      {
        id: 1,
        text: "Doctor Appointment",
        day: "March 1 at 2.30",

        reminder: true,
      },
      {
        id: 2,
        text: "Meeting at School",
        day: "March 1 at 2.30",

        reminder: true,
      },
      {
        id: 3,
        text: "School Shopping",
        day: "March 1 at 2.30",

        reminder: true,
      },
    ];
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
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
