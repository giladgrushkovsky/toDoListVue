<template>
  <div class="addTaskContainer">
    <h2 class="red">Add Task:</h2>
    <div>
      <textarea
        class="inputText"
        @keyup="keyUp"
        key="addTaskText"
        placeholder="Type here to add task and hit 'Enter'"
      ></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: "addTask",
  props: {
    taskList: Array,
  },
  emits: ["addTask"],
  methods: {
    keyUp: function (event) {
      if (event.keyCode == 13 && event.target.value.trim().length != 0) {
        this.$emit("addTask", event.target.value);
        event.target.value = "";
      }
    },
  },
};
</script>

<style scoped>
.addTaskContainer {
  padding: 5px;
  margin: 5px;
  width: 70%;
  border-radius: 10px;
  position: relative;
  min-height: 40px;
  text-align: left;
  margin-left: auto;
  margin-right: auto;
}
.inputText {
  width: 100%;
}
</style>
