<template>
  <div class="home">
    <Header/>
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Header from "../components/layout/Header";
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";

let request = indexedDB.open("TodoList", 1);
export default {
  name: "home",
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
      const open = indexedDB.open("TodoList", 4);
      open.onsuccess = e => {
        let db = e.target.result;
        let deleteData = db
          .transaction(["todos"], "readwrite")
          .objectStore("todos")
          .delete(id);
        deleteData.onsuccess = e => {
          console.log("Data Deleted");
        };
      };
    },
    addTodo(newTodo) {
      this.todos = [...this.todos, newTodo];
      const open = indexedDB.open("TodoList", 4);
      open.onsuccess = e => {
        let db = e.target.result;
        let transaction = db.transaction(["todos"], "readwrite");

        transaction.onsuccess = e => {
          console.log("transaction done...");
        };
        transaction.onerror = e => {
          console.log("ERROR: " + e.target.errorCode);
        };

        let os = transaction.objectStore("todos");
        let addData = os.add(newTodo);
        addData.onsuccess = e => {
          location.reload();
        };
      };
    }
  },
  beforeCreate() {
    if (!indexedDB) {
      alert("Your browser doesn't support IndexedDB.");
    }
    const open = indexedDB.open("TodoList", 4);

    open.onupgradeneeded = e => {
      let db = e.target.result;
      if (!db.objectStoreNames.contains("todos")) {
        let store = db.createObjectStore("todos", {
          keyPath: "id",
          autoIncrement: true
        });
        let index = store.createIndex("title", "title", { unique: false });
      }
      db.close();
    };

    open.onerror = e => {
      console.log("Database Error: " + e.target.errorCode);
    };

    open.onsuccess = e => {
      let db = e.target.result;
      db.close();
    };
  },
  created() {
    const open = indexedDB.open("TodoList", 4);
    open.onsuccess = e => {
      let db = e.target.result;
      let os = db.transaction("todos").objectStore(["todos"], "readonly");
      os.openCursor().onsuccess = e => {
        let cursor = e.target.result;
        if (cursor) {
          this.todos.push(cursor.value);
          cursor.continue();
        } else {
          db.close();
        }
      };
    };
  }
};
</script>
