<template>
  <div id="app">
    <header class="header">
      <h1>ToDoリスト</h1>
    </header>

    <form class="radio_button">
      <input
        type="radio"
        name="syurui"
        checked="checked"
        id="radio-all-select"
        onchange="radioChange()"
        value="all"
        v-model="picked"
      />すべて
      <input
        type="radio"
        name="syurui"
        id="adio-working-select"
        onchange="radioChange()"
        value="doing"
        v-model="picked"
      />作業中
      <input
        type="radio"
        name="syurui"
        id="radio-done-select"
        onchange="radioChange()"
        value="done"
        v-model="picked"
      />完了
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
    };
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
  // computed:{
  //   picked:function(){

  //   }
  // }
};
</script>
