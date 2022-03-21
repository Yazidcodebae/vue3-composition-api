<template>
  <div class="container mt-20px">
    <div class="row">
      <div class="col-xxl-4">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">SIMPLE TODO APP</h5>
            <div class="card-form">
              <div class="row">
                <div class="col-8">
                  <input
                    type="text"
                    class="form-control"
                    v-model="todo"
                    @keyup.enter="addTodo"
                  />
                </div>
                <div class="col-4">
                  <button class="btn btn-success" @click="addTodo">ADD</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <list
        :todos="list"
        @deleteTodo="deleteTodo"
        @doneTodo="doneTodo"
        @undoneTodo="undoneTodo"
      />
      <small class="text-start">Total Todo : {{ totalTodo }}</small>
    </div>
  </div>
</template>

<script>
import { ref, reactive, onMounted, computed, toRefs } from "vue";
import List from "./components/List.vue";
export default {
  name: "App",
  components: { List },
  setup() {
    const todo = ref("");
    const todos = reactive({
      list: [],
    });
    onMounted(() => {
      const items = localStorage.getItem("todos");
      todos.list = items ? JSON.parse(items) : [];
    });
    const totalTodo = computed(() => {
      return todos.list.length;
    });

    const addTodo = () => {
      // this.todos.push(this.todo); // menambahkan data di akhir array
      todos.list.unshift({
        activity: todo.value,
        isDone: false,
      });
      todo.value = "";
      saveToLocalStorage();
    };
    const deleteTodo = todoIndex =>{
      todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      saveToLocalStorage();
    };
    const doneTodo = todoIndex =>{
      todos.list = todos.list.filter((item,index) => {
        if (index == todoIndex) {
          item.isDone = true;
        }
        return item;
      })
      saveToLocalStorage();
    }
    const undoneTodo = todoIndex => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = false;
        }
        return item;
      });
      saveToLocalStorage();
    }
    const saveToLocalStorage = () => {
      localStorage.setItem("todos", JSON.stringify(todos.list));
    }
    return { todo, ...toRefs(todos), totalTodo,addTodo,deleteTodo,doneTodo,undoneTodo };
  },
};
</script>

