<template>
  <div class="about">
    <Header/>
    <section>
      <h1>LAGS TodoList App</h1>
      <p>This is a simple to-do app with Vue.js, and IndexedDB.</p>
    </section>
  </div>
</template>
<script>
import Header from "../components/layout/Header";
export default {
  name: "About",
  components: {
    Header
  }
};
</script>

<style scoped>
section {
  padding: 1rem;
}
</style>
