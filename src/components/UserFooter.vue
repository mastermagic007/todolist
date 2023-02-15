<template>
    <div class="todo-footer" v-show="totalItem">
        <label>
        <input type="checkbox" v-model:checked="isAll"/>
        </label>
        <span>
        <span>已完成{{itemChecked}}</span> / 全部{{totalItem}}
        </span>
        <button class="btn btn-danger" @click="clearFinishItem">清除已完成任务</button>
    </div>
</template>

<script>
export default {
    name:'UserFooter',
    props:['todos','checkAll','clearFinish'],
    computed:{
      itemChecked(){
        //筛选出是否被选中的元素个数
        return this.todos.reduce((pre,current)=>{
          //pre的值跟第二参数进行绑定 current 代表遍历时的当前元素
          return pre + (current.done ? 1 : 0)
        },0)
      },
      //全部元素的个数
      totalItem(){
        return this.todos.length
      },
      isAll:{
        get(){
          return this.itemChecked === this.totalItem && this.totalItem > 0
        },
        set(value){
          //当but按钮被设置新值时，调用方法将全部按钮状态改为true
          this.checkAll(value)
        }
      }
    },
    methods:{
      clearFinishItem(){
          this.clearFinish()
      }
    }
}
</script>

<style scoped>
    /*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }

  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }

  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }

  .todo-footer button {
    float: right;
    margin-top: 5px;
  }

</style>