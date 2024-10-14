  <template>
  <div class="app-container">
    <h2>Lista zadań</h2>

    <form @submit.prevent="addTask" class="add-task">
      <input type="text" v-model="newTask" placeholder="Dodaj nowe zadanie">
      <button type="submit">Dodaj</button>
    </form>

    <ul class="task-list">
      <todoItem
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        @remove="removeTask"
        @update="updateTask"
      />
    </ul>
  </div>
</template>

<script>
  import { ref } from "vue";
  import TodoItem from "./components/TodoItem.vue"

  export default {
    components: {
      TodoItem
    },
    setup() {
      const newTask = ref("");
      const tasks = ref([]);

      const addTask = () => {
        if (newTask.value.trim()) {
          tasks.value.push({
            id: Date.now(),
            text: newTask.value.trim()
          });
        }
        newTask.value = "";
      }
      const removeTask = (taskId) => {
        tasks.value = tasks.value.filter(task => task.id !== taskId);
      }
      const updateTask = (updateTask) => {
        const index = tasks.value.findIndex(task => task.id === updateTask.id);

        if (index !== -1) {
          tasks.value[index].text = updateTask.text;
        }
      }
      return {
        newTask,
        tasks,
        addTask,
        removeTask,
        updateTask
      }
    }
  }
</script>

<style scoped>
  .app-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 0 auto;
    padding: 20px;
    height: 50vh;
    background-color: white;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
    font-family: Verdana, Geneva, Tahoma, sans-serif;
  }

  .addTask {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }

  .addTask input {
    flex: 1;
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }

  button {
    padding: 5px 10px;
    margin-left: 10px;
    background-color: #428883;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  button:hover {
    background-color: #35495E;
  }

  .task-list {
    padding: 0;
    margin: 0;
    list-style-type: none;
    overflow-y: auto;
  }
</style>
