<template>
  <div class="container">
    <Header
      v-on:toggle-add-task="toggleAddTask()"
      title="Task Tracker"
      v-bind:showAddTask="showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask v-on:add-task="addTask" />
    </div>

    <!--here is the parent function that will do something when the childs emit their call -->
    <Tasks
      v-on:toggle-reminder="toggleReminder"
      v-on:delete-task="deleteTask"
      v-bind:tasks="tasks"
    />
  </div>
</template>

<script>
import Header from "./components/Header";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    addTask(task) {
      this.tasks = [...this.tasks, task];
    },
    deleteTask(id) {
      //confirm functions as an alert; it pops up a message with a 'yes' or 'no' choice.
      if (confirm("Are you sure you want to delete it?")) {
        //i want everything back except the id that i clicking on. That is why task id shouldn't match id
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    toggleReminder(id) {
      //if the id i'm clicking equals the one i'm passing so bring all the task charasteristics (...task) and change the reminder attribute if diff from the one already set. If not, no change so bring the task as is.
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: !task.reminder } : task
      );
    },
  },
  created() {
    //this method will fill in the tasks array above every time is invoked.
    this.tasks = [
      {
        id: 1,
        text: "Doctors Appointment",
        day: "March 1",
        reminder: true,
      },
      {
        id: 2,
        text: "Work Meeting",
        day: "April 12",
        reminder: true,
      },
      {
        id: 3,
        text: "Party at Susan",
        day: "December 24",
        reminder: false,
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
