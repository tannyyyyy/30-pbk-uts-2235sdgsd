<template>
  <div id="app" class="container">
  <h1>
     Tanny Apriliani 
    </h1>
    <img src="./assets/keranjang2.png" class="logo">
    <TodoList/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import TodoList from './components/TodoList'
export default {
  name: 'App',
  components: {
    HelloWorld,
    TodoList
  }
}
</script>

<style>
* {color: black;
  background-color:burlywood;
  box-sizing: border-box;
}
.container {
  max-width: 600px;
  margin: 0 auto;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 24px;
}
.logo {
  display: block;
  margin: 20px auto;
  height: 75px;
}
</style>
