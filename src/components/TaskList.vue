<template>
  <div class="toDoListContainer">
    <div id="filter">
      show :
      <select :value="filter" @change="filterChange">
        <option value="-1">All</option>
        <option value="0">Pending</option>
        <option value="1">Completed</option>
      </select>
    </div>
    <div id="emptyList" v-if="workTaskList?.length == 0">no items found</div>
    <div
      class="task"
      v-for="task in workTaskList"
      :key="task.taskId"
      :class="{ done: task.status }"
    >
      <span>
        <input
          type="checkbox"
          :checked="task.status"
          @click="updateStatus(task)"
          :id="task.taskId"
          :key="'check' + task.taskId"
        />
      </span>
      {{ task.text }}
      <div class="remove">
        <span @click="removeItem(task)"> remove</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskList",
  props: {
    taskList: Array,
  },
  data() {
    return {
      filter: -1,
    };
  },
  emits: ["removeItem", "updateStatus"],
  methods: {
    removeItem: function (item) {
      this.$emit("removeItem", item);
    },
    updateStatus: function (task) {
      this.$emit("updateStatus", task.taskId);
    },
    filterChange: function (event) {
      this.filter = event.target.value;
    },
  },
  computed: {
    workTaskList: function () {
      if (this.filter == -1) {
        return this.taskList;
      } else {
        return this.taskList.filter((t) => t.status == this.filter);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.toDoListContainer {
  width: 70%;
  margin-left: auto;
  margin-right: auto;
  text-align: left;
}

#filter {
  padding-bottom: 10px;
}
.task {
  border: solid 1px black;
  padding: 5px;
  margin: 5px;
  border-radius: 10px;
  position: relative;
  min-height: 40px;

  .remove {
    position: absolute;
    right: 10px;
    top: 10%;
    background-color: rgb(241, 64, 64);
    padding: 5px;
    color: white;
    border-radius: 10px;
    display: none;
    cursor: pointer;
  }
}
.task.done {
  text-decoration: line-through;
  opacity: 0.6;
}

.task:hover {
  background-color: antiquewhite;
  .remove {
    display: block;
  }
}
</style>
