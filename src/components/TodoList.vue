<template>
  <div class="todo-list">
    <h1>Todo List</h1>
    <input v-model="newTodo" placeholder="Add a new task" @keyup.enter="addTodo" />
    <button @click="addTodo">Add</button>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <span :class="{ completed: todo.completed }" @click="toggleTodoCompletion(index)">
          {{ todo.text }}
        </span>
        <button @click="deleteTodo(index)">Delete</button>
      </li>
    </ul>
    <p>{{ incompleteTodos }} tasks remaining</p>
  </div>
</template>

<script>
import { useTodoStore } from '../stores/todoStore';
import { computed, ref } from 'vue';

export default {
  setup() {
    const store = useTodoStore();
    const newTodo = ref('');

    const addTodo = () => {
      if (newTodo.value.trim()) {
        store.addTask(newTodo.value);
        newTodo.value = '';
      }
    };

    const deleteTodo = (index) => {
      store.removeTask(index);
    };

    const toggleTodoCompletion = (index) => {
      store.toggleTaskStatus(index);
    };

    return {
      newTodo,
      todos: computed(() => store.tasks),
      addTodo,
      deleteTodo,
      toggleTodoCompletion,
      incompleteTodos: computed(() => store.incompleteTasksCount),
    };
  },
};
</script>

<style scoped>
.todo-list {
  max-width: 400px;
  margin: 50px auto; /* Margin atas dan bawah 50px, dan secara otomatis berada di tengah */
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background-color: #fff4e0; /* Warna latar belakang */
  font-family: 'Poppins', sans-serif; /* Menggunakan font Poppins */
  text-align: center; /* Mengatur tampilan ditengah */
}

h1 {
  color: #333; /* Warna teks judul */
}

input {
  width: calc(100% - 70px);
  padding: 10px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  background-color: #c97f72; /* Warna tombol */
  color: #fff;
  cursor: pointer;
}

button:hover {
  background-color: #9f0d0d; /* Warna tombol saat dihover */
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid #ccc;
}

li .completed {
  text-decoration: line-through;
  color: #999; /* Warna teks tugas yang sudah selesai */
}

span {
  cursor: pointer;
}
</style>
