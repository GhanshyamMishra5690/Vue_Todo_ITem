<template>
    <main class="app">
        <section class="greeting">
            <h2 class="title">
                What's up,
                <input type="text" placeholder="Name here" v-model="name" />
            </h2>
        </section>
        <section class="create-todo">
            <h3>CREATE A TODO</h3>
            <form @submit.prevent="addTodo">
                <h2>What's on your todo list</h2>
                <input
                    type="text"
                    placeholder="make a video"
                    v-model="input_content"
                />
               
                <h4>Pick a Category</h4>
                <div class="options">
                    <label>
                        <input
                            type="radio"
                            name="category"
                            id="category1"
                            value="business"
                            v-model="input_category"
                        />
                        <span class="bubble business"></span>
                        <div>Business</div>
                    </label>

                    <label>
                        <input
                            type="radio"
                            name="category"
                            id="category1"
                            value="personal"
                            v-model="input_category"
                        />
                        <span class="bubble personal"></span>
                        <div>Personal</div>
                    </label> 
                </div>
                <input type="submit" value="Add todo" />
            </form>
        </section>
        <section class="todo-list">
          <h3>TODO LIST</h3>
            <div class="list">
              <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`" :key="todo">
                <label>
                    <input type="checkbox" v-model="todo.value" />
                    <span :class="`bubble ${todo.category}`"></span>
                </label>

                <div class="todo-content">
                   <input type="text" v-model="todo.content" />
                </div>

                <div class="actions">
                  <button class="delete" @click="removeTodo(todo)">Delete</button>
                </div>
              </div>
            </div>
        </section>
    </main>
</template>

<script setup>
import { ref, computed, watch, onMounted } from "vue";
const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_category = ref(null);
const todos_asc = computed(() => {
    const sortedTodos = [...todos.value];
    // Sort the copied array based on createdAt property
    return sortedTodos.sort((a, b) => a.createdAt - b.createdAt);
});

const addTodo = () => {
  if(input_content.value.trim() === '' || input_category.value === null){
    return ;
  }
  todos.value.push({
    content:input_content.value,
    category:input_category.value,
    done:false,
    createdAt: new Date().getTime()
  })  
  input_content.value =''
  input_category.value = null
}
const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep:true })

watch(name, (newVal) => {
    localStorage.setItem("name", newVal);
});
onMounted(() => {
    name.value = localStorage.getItem("name");
    todos.value = JSON.parse(localStorage.getItem('todos'));
});



console.log(input_category);
console.log(input_content);
console.log(todos_asc);
</script>
