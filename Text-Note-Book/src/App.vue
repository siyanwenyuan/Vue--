<script>
export default {
  //写一个响应式数据
  data(){
    return{
      //设置一些静态值
      todos:
      [
        {
          id: 123123,
          title: '测试内容1',
          completed: false
        },
        {
          id: 22323,
          title: '测试内容2',
          completed: true
        },
      ],
      visibility: 'all',

    }

  },

  methods:{
    toggleAll(e){
      this.todos.forEach(todo=>todo.completed=e.target.checked)
    },
    onHashChange(){
    const hash=window.location.hash.replace(/#\/?/,'')
      if(['all','active','completed'].includes(hash)){
        this.visibility=hash

      }else{
        window.location.hash=null;
        this.visibility='all'
      }

    },
    //e是这个事件对象
    addTodo(e){
      const title=e.target.value.trim()
      if(!title){
        return
      }
      this.todos.push({
        id: Date.now(),
        title,
        completed: false
      })
      //输入完毕后，需要清空
      e.target.value=''
    },

    removeTodo(todo){
      this.todos.splice(this.todos.indexOf(todo),1)
    },
    //清除已经完成的
    clearCompleted(){
      return this.todos.filter(todo=>!todo.completed)
    }


  },

  computed: {
    filteredTodos(){
      switch (this.visibility){
        case 'all':
          return this.todos
        case 'active':
          return this.todos.filter(todo=>!todo.completed)
        case 'completed':
          return this.todos.filter(todo=>todo.completed)
      }

    },
    remaining(){
      return this.todos.filter(todo=>!todo.completed).length
    }
  },
  mounted() {
    window.addEventListener('hashchange',this.onHashChange)
    this.onHashChange()
  }
}
</script>

<template>


  <section class="todoapp">
    <header class="header">
      <h1>todos</h1>
      <input @keyup.enter="addTodo" class="new-todo" placeholder="What needs to be done?" autofocus>
    </header>
    <!-- This section should be hidden by default and shown when there are todos -->
    <section class="main">
      <input id="toggle-all" class="toggle-all" type="checkbox" @click="toggleAll">
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <!-- These are here just to show the structure of the list items -->
        <!-- List items should get the class `editing` when editing and `completed` when marked as completed -->
        <li
            v-for="todo in filteredTodos"
            :key="todo.id"
            :class="{completed: todo.completed}">
          <div class="view">
            <input class="toggle" type="checkbox" checked v-model="todo.completed">
            <label v-text="todo.title"></label>
            <button class="destroy" @click="removeTodo(todo)"></button>
          </div>
          <input class="edit" value="Create a TodoMVC template">
        </li>
      </ul>
    </section>
    <!-- This footer should be hidden by default and shown when there are todos -->
    <footer class="footer">
      <!-- This should be `0 items left` by default -->
      <span class="todo-count"><strong>{{remaining}}</strong> item left</span>
      <!-- Remove this if you don't implement routing -->
      <ul class="filters">
        <li>
          <a :class="{selected: visibility==='all'}" href="#/all">All</a>
        </li>
        <li>
          <a :class="{selected: visibility==='active'}"  href="#/active">Active</a>
        </li>
        <li>
          <a :class="{selected: visibility==='completed'}"  href="#/completed">Completed</a>
        </li>
      </ul>
      <!-- Hidden if no completed items are left ↓ -->
      <button class="clear-completed" @click="clearCompleted">Clear completed</button>
    </footer>
  </section>
</template>

<style>
@import "https://unpkg.com/todomvc-app-css@2.4.1/index.css";
</style>
