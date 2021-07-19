<template>
  <div id="app">
    <header class="header">
      <h1>ToDoリスト</h1>
    </header>

    <form class="radio_button">
      <input type="radio" name="select" value="all" v-model="picked" />すべて
      <input type="radio" name="select" value="doing" v-model="picked" />作業中
      <input type="radio" name="select" value="done" v-model="picked" />完了
    </form>
    <table>
      <thead>
        <th>ID</th>
        <th>コメント</th>
        <th>状態</th>
      </thead>
      <tbody>
        <tr v-for="(todo, index) of filterring(picked)" :key="index">
          <td>{{ todo.id }}</td>
          <td>{{ todo.comment }}</td>
          <td>
            <button v-on:click="statusBtn(todo)">
              {{ todo.status }}
            </button>
          </td>
          <td><button v-on:click="deleteBtn(index)">削除</button></td>
        </tr>
      </tbody>
    </table>

    <h2>新規タスクの追加</h2>

    <input id="input-todo" type="text" v-model="newItem" />
    <button id="add-button" @click="addItem">追加</button>
  </div>
</template>
<script>
export default {
  name: 'app',
  data() {
    return {
      newItem: '',
      todos: [],
      todo: {},
      addTask: '',
      picked: 'all',
    };
  },
  methods: {
    addItem: function() {
      if (this.newItem == '') return;
      this.todo = {
        id: this.todos.length,
        comment: this.newItem,
        status: '作業中',
      };
      this.todos.push(this.todo);
      this.newItem = '';
    },
    deleteBtn: function(index) {
      this.todos.splice(index, 1);
      this.todos.reduce((Idnum, todo) => (todo.id = Idnum + 1), -1);
    },
    statusBtn: function(todo) {
      if (todo.status === '作業中') {
        todo.status = '完了';
      } else {
        todo.status = '作業中';
      }
    },
  },
  computed: {
    filterring() {
      return function() {
        if (this.picked === 'all') {
          return this.todos;
        } else if (this.picked === 'doing') {
          return this.todos.filter((todo) => {
            return todo.status === '作業中';
          });
        } else if (this.picked === 'done') {
          return this.todos.filter((todo) => {
            return todo.status === '完了';
          });
        }
      };
    },
  },
};
</script>
