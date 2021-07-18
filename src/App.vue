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
        <tr v-for="(todo, index) in todos" :key="index">
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
    <p>{{ todos }}</p>
    <p>{{ picked }}</p>

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
      addTask: '',
      picked: 'all',
    };
  },
  watch: {
    picked: function() {
      if (this.picked === 'all') {
        this.todos.slice();
        console.log(this.picked);
      } else if (this.picked === 'doing') {
        console.log(this.picked);
        this.todos.filter((todo) => {
          return todo.status === '作業中';
        });
      } else if (this.picked === 'done') {
        console.log(this.picked);
        this.todos.filter((todo) => {
          return todo.status === '完了';
        });
      }
    },
  },
  methods: {
    addItem: function() {
      if (this.newItem == '') return; //タスク未入力の場合は追加しない
      let todo = {
        id: this.todos.length,
        comment: this.newItem,
        status: '作業中',
      };
      this.todos.push(todo);
      this.newItem = ''; //タスク追加後に入力欄を空にする
    },
    deleteBtn: function(index) {
      //indexを引数に指定
      this.todos.splice(index, 1); //indexで指定された要素を1つ削除
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
};
</script>
