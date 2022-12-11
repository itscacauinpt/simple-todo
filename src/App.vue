<script setup lang="ts">
import { onMounted, ref, watch, computed } from 'vue';

const name: any = ref('')
const todos: any = ref([])

const errorTask: any = ref('')

const input_content: any = ref('')
const input_category: any = ref(null)

watch(name, (newName: any) => localStorage.setItem('name', newName))

watch(todos, (newTodo: any) => {
  localStorage.setItem('todos', JSON.stringify(newTodo))
}, {
  deep: true
})

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    console.log('deal with error')
    errorTask.value = 'error: add a task'
    return
  } else {
    errorTask.value = ''
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime(),
  })
}

const removeTodo = (todo: any) => todos.value = todos.value.filter((ele: any) => ele !== todo)

const todos_asc = computed(() => todos.value.sort((a: any, b: any) => a.createdAt - b.createdAt))

onMounted(() => {
  const name_local_storage: any = localStorage.getItem('name')
  const todos_local_storage: any = localStorage.getItem('todos')

  name.value = name_local_storage || ''
  todos.value = JSON.parse(todos_local_storage) || []
})

</script>

<template>
  <main class="app">

    <section class="greetings">
      <h2 class="title">
        What's up,
        <input type="text" id="name" placeholder="your name" v-model="name">
      </h2>
    </section>

    <section class="create-todo">
      <h2>create todo</h2>

      <form id="new-todo-form" @submit.prevent="addTodo">
        <h4>here we go again</h4>
        <input
          type="text"
          name="content"
          id="content"
          placeholder="finish that project"
          v-model="input_content"
        />
        <h4>pick a category</h4>
        <div class="options">

          <label>
            <input
              type="radio"
              name="category"
              id="category1"
              value="business"
              v-model=input_category
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="category"
              id="category2"
              value="personal"
              v-model=input_category
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>

        </div>

        <input
          type="submit"
          value="add todo"
        />

        <span>{{errorTask}}</span>
      </form>
    </section>

    <section class="todo-list">
      <h3>todo list</h3>
      <div class="list" id="todo-list">
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">

          <label>
            <input
              type="checkbox"
              v-model="todo.done"
            />
            <span :class="`bubble ${
							todo.category == 'business' 
								? 'business'
								: 'personal'
						}`"></span>
          </label>

          <div class="todo-content">
            <input
              type="text"
              v-model="todo.content"
            />
          </div>

          <div class="actions">
            <button
              class="delete"
              @click="removeTodo(todo)"
            >
              Delete
            </button>

          </div>

        </div>
      </div>
    </section>

  </main>
</template>
