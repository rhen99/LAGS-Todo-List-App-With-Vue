<template>
  <div>
    <div class="todo-item" v-bind:class="{'is-complete': todo.completed}">
      <p>
        {{todo.title}}
        <input type="checkbox" @change="markComplete" name="check" v-model="check">
      </p>
      <button class="del" @click="$emit('del-todo', todo.id)">x</button>
    </div>
  </div>
</template>
<script>
export default {
  name: "TodoItem",
  props: ["todo"],
  data() {
    return {
      check: false
    };
  },
  methods: {
    markComplete() {
      this.todo.completed = this.check;
      const open = indexedDB.open("TodoList", 4);
      open.onsuccess = e => {
        let db = e.target.result;
        let os = db.transaction(["todos"], "readwrite").objectStore("todos");
        let getData = os.get(this.todo.id);

        getData.onerror = e => {
          console("Can't get data");
        };

        getData.onsuccess = e => {
          let data = e.target.result;

          data.completed = this.todo.completed;

          let update = os.put(data);

          update.onsuccess = e => {
            console.log("Update Successfully");
          };
          update.onerror = e => {
            console.log("Update Denied");
          };
        };
      };
    }
  }
};
</script>
<style scoped>
.todo-item {
  position: relative;
  background-color: #f4f4f4;
  border-bottom: 1px solid #333;
  padding: 15px;
}
.del {
  position: absolute;
  left: 95%;
  bottom: 35%;
  font-size: 0.8rem;
  background: rgb(250, 27, 27);
  color: #fff;
  border: 0;
  outline: 0;
  padding-bottom: 5px;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  cursor: pointer;
}
.is-complete {
  text-decoration: line-through;
}
</style>

