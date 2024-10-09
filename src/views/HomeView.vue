<template>
  <div>
    <Banner />
    <div class="main-container">
      <h1>Cadastre uma nova tarefa:</h1>
      <TaskForm />
    </div>
  </div>
</template>

<script>
  import Banner from '@/components/Banner.vue';
  import TaskForm from '@/components/Formulario.vue';

  export default {
    name: "Home",
    components: {
      Banner,
      TaskForm
    }
  }
</script>

<style scoped>
  .main-container {
    padding: 20px;
    max-width: 800px;
    margin: 0 auto;
  }

  h1 {
    text-align: center;
    margin-bottom: 20px;
  }
</style>
