<script setup lang="ts">
import { ref, onMounted, computed, watch } from 'vue'

const name = ref('')
const todos = ref([])

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))

</script>

<template>

  <main class="app">

    <section class="greetings">

      <h2 class="title">
 
      </h2>

    </section>

  </main>

</template>