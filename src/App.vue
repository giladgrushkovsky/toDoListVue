<template>
  <div id="app">
    <AddTask @addTask="addTask" />
    <ToDoList
      :taskList="taskList"
      @removeItem="removeItem"
      @updateStatus="updateStatus"
    />
  </div>
</template>

<script>
import ToDoList from "./components/TaskList.vue";
import AddTask from "./components/AddTask.vue";

export default {
  name: "app",
  components: {
    ToDoList,
    AddTask,
  },
  methods: {
    addTask: function (text) {
      this.taskList.push({ taskId: this.currenttaskId, status: false, text });
      this.currenttaskId = this.currenttaskId + 1;
      this.saveToLocalStorage();
    },
    removeItem: function (item) {
      this.taskList.splice(this.taskList.indexOf(item), 1);
      this.saveToLocalStorage();
    },
    updateStatus: function (taskId) {
      const taskDetail = this.getTask(taskId);
      if (taskDetail && taskDetail.task) {
        taskDetail.task.status = !taskDetail.task.status;
        this.taskList[taskDetail.index] = taskDetail.task;
      }
      this.saveToLocalStorage();
    },
    getTask: function (id) {
      let index = 0;
      let task = null;
      for (index = 0; index < this.taskList.length && !task; index++) {
        if (this.taskList[index].taskId == id) {
          task = this.taskList[index];
        }
      }
      return { index: index - 1, task };
    },
    saveToLocalStorage: function () {
      const saveData = {
        taskList: this.taskList,
        currenttaskId: this.currenttaskId,
      };
      localStorage.setItem("taskList", JSON.stringify(saveData));
    },
    getFromLocalStorage: function () {
      let data = { taskList: [], currenttaskId: 0 };
      try {
        const localData = JSON.parse(localStorage.getItem("taskList"));
        if (localData && localData.taskList) {
          data = localData;
        }
      } catch (e) {
        console.log(e);
      }
      return data;
    },
  },

  data() {
    return this.getFromLocalStorage();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
