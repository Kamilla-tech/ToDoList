<template>
 <li class="task-item">
   <input type="text" v-model="localTaskText" @blur="updateTask">
   <button @click="removeTask">Usuń</button>
 </li>
</template>

<script>
  import { ref, watch } from "vue";

  export default {
    props: ["task"],
    emits: ["remove", "update"],
    setup(props, { emit }) {
      const localTaskText = ref(props.task.text);

      watch(localTaskText, (newValue) => {
        emit("update", { id: props.task.id, text: newValue });
      });

      const updateTask = () => {
        emit("update", { id: props.task.id, text: localTaskText.value });
      };

      const removeTask = () => {
        emit("remove", props.task.id);
      };

      return {
        localTaskText,
        updateTask,
        removeTask
      }
    }
  }
</script>

<style scoped>
  .task-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 10px;
  }

  button {
    padding: 5px 10px;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  button:hover {
    background-color: #d32f2f;
  }

 input {
  flex: 1;
   margin-right: 10px;
   padding: 5px;
   border: 1px solid #ccc;
   border-radius: 5px;
 }
</style>
