<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <UserTop :addTodo="addTodo"></UserTop>
        <UserList :todos="todos" :checkTodo="checkTodo" :delTodo="delTodo"></UserList>
        <UserFooter :todos="todos" :checkAll="checkAll" :clearFinish="clearFinish"></UserFooter>
      </div>
    </div>
  </div>
</template>

<script>
  import UserTop from './components/UserTop'
  import UserList from './components/UserList'
  import UserFooter from './components/UserFooter'

  export default {
    name: 'App',
    components: {
      UserTop,
      UserList,
      UserFooter
    },
    data(){
      return {
          todos:JSON.parse(localStorage.getItem('todos')) || []
      }
    },
    methods:{
      //添加元素
      addTodo(todoObj){
        this.todos.unshift(todoObj)
      },
      //取消/勾选 状态
      checkTodo(id){
        this.todos.forEach((todo)=>{
          if(todo.id === id) todo.done =! todo.done
        })
      },
      //删除元素
      delTodo(id){
        this.todos = this.todos.filter((todo)=>{
          return todo.id !== id
        })
      },
      //item全选按钮
      checkAll(done){
        this.todos.forEach((todo)=>{
          todo.done = done
        })
      },
      //清楚已经完成的项目
      clearFinish(){
        //过滤掉已经选中的item
        return this.todos = this.todos.filter((todo)=>{
          return !todo.done
        })
      }
    },
    watch:{
      todos:{
        deep:true,//监视done值的变化，变化了也需要将本地存储数据进行更新
        handler(value){
          localStorage.setItem('todos',JSON.stringify(value))
        }
      }
    }
  }
</script>

<style>
  /*base*/
  body {
    background: #fff;
  }

  .btn {
    display: inline-block;
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
  }

  .btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
  }

  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }

  .btn:focus {
    outline: none;
  }

  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }

</style>
