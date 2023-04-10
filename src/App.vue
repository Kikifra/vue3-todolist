<script setup>
import { ref, computed, watch, onMounted} from 'vue';

const name = ref('')
const todos = ref([])

const inputContent = ref('')
const inputCategory = ref(null)

const compareTodo = (a, b) => {return a.createAt - b.createAt};
const todosAsc = computed(() => todos.value.sort(compareTodo));
//
watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
});

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal));
}, { deep: true });

//
const addTodo = () => {
  if(inputContent.value.trim() === '' || inputCategory.value === null){
    return
  }

  todos.value.push({
    content: inputContent.value,
    category: inputCategory.value,
    edit: false,
    done: false,
    createdAt: new Date().getTime()
  })

};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => {t !== todo});
};


onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
});
</script>

<template>
  <main class="app">

    <section class="greeting">
      <h2 class="title">
        hello, <input type="text" placeholder="name here"
        v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>创建计划：</h3>
      <form @submit.prevent="addTodo">
        <input 
          type="text"
          placeholder="输入你的计划"
          v-model="inputContent"
          />
          <h4>选择样式：</h4>

          <div class="options">
            <label>
              <input
                type="radio"
                name="category"
                value="business"
                v-model="inputCategory"
              />
              <span class="bubble business"></span>
              <!-- <div>blue</div> -->
            </label>

            <label>
              <input
                type="radio"
                name="category"
                value="personal"
                v-model="inputCategory"
              />
              <span class="bubble personal"></span>
              <!-- <div>pink</div> -->
            </label>
            <label>
              <input
                type="radio"
                name="category"
                value="another"
                v-model="inputCategory"
              />
              <span class="bubble another"></span>
              <!-- <div>green</div> -->
            </label>
            <label>
              <input
                type="radio"
                name="category"
                value="another2"
                v-model="inputCategory"
              />
              <span class="bubble another2"></span>
              <!-- <div>yellow</div> -->
            </label>
          </div>

          <input type="submit" value="添加计划">
      </form>
    </section>

    <section class="todo-list">
      <h3>计划项目</h3>
      <div class="list" id="todo-list">
        <div v-for="todo in todosAsc" :class="`todo-item ${todo.done && 'done'}`">
            <label>
              <input type="checkbox" v-model="todo.done" />
              <span :class="`bubble ${
                todo.category == 'business' 
                  ? 'business' 
                  : todo.category == 'personal' 
                          ? 'personal' 
                          : todo.category == 'another' ? 'another' : 'another2'
              }`">
              </span>
            </label>

            <div class="todo-content">
              <input type="text" v-model="todo.content" />
            </div>

            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">删除</button>
            </div>

        </div>

      </div>
    </section>

  </main>
</template>
