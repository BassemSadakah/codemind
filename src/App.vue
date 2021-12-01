<template>
  <div class="w-full h-full relative">
    <Todo/>
  </div>
</template>

<script>
import Todo from './components/Todo.vue'
export default {
  name: 'App',
  components: {
    Todo,
  }
}
</script>

<style>
body{
  background: #F9FAFB;

}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 160px; */
  width: 100%;
  height: 100%;
}
</style>
