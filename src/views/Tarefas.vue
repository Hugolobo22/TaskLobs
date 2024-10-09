<template>
  <div class="main-container">
    <h1>Lista de Tarefas:</h1>
    <Dashboard />
  </div>
</template>

<script>
import Dashboard from '@/components/Dashboard.vue';

export default {
  name: "Tarefas",

  components: {
    Dashboard
  }
}
</script>

<style scoped>
  .main-container {
    padding: 20px;
    max-width: 1000px;
    margin: 0 auto;
  }

  h1 {
    text-align: center;
    margin-bottom: 20px;
  }
</style>
