<template>
  <header>
    <h1>LAGS Todo List</h1>
    <div id="nav">
      <router-link to="/">Home</router-link>|
      <router-link to="/about">About</router-link>
    </div>
  </header>
</template>
<script>
export default {
  name: "Header"
};
</script>
<style scoped>
header {
  background-color: #333;
  color: #fff;
  text-align: center;
  padding: 10px;
}
header a {
  text-decoration: none;
  color: #fff;
  padding-right: 5px;
}
</style>

