<template>
<!-- 当已完成数和全部数量都为0时勾选没有意义，可以使其不显示。total为0则为假，不展示-->
    <div class="todo-footer" v-show="total">
        <label>
            <input type="checkbox" :checked="isAll" @change="checkAll" >
            <!-- :checked="iaAll"表示，当已完成数和全部数量相等时自动勾选,但当两者都为0时自动勾选没有意义，可以使其不显示-->
            <!-- @change="checkAll"点击全选或全不选 -->
           <!-- <input type="checkbox" :checked="isAll" @change="checkAll" v-model="isAll" >和下面的getter和setter是另一种写法 -->
        </label>
        <span>
            <span>已完成{{doneTotal}}</span>/全部{{total}}
        </span>
        <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
    </div>
</template>

<script>
export default {
    name:"MyFooter",
    props:["todos","checkAllTodo","clearAllTodo"],
    computed:{
        doneTotal(){
          return  this.todos.reduce((pre,current)=>pre+(current.done?1:0),0)
        },
        total(){
            return this.todos.length
        },
        isAll(){
            //满足下面两个条件直接返回布尔值
            return this.doneTotal===this.todos.length && this.total>0  
        }
// 或者：整合在一起
// isAll:{
//     get(){
//         return this.doneTotal===this.todos.length && this.total>0  
//     },
//     set(value){
//         checkAll(e){
//             this.checkAllTodo(e.target.checked)  
//         }
//     }
// }

    },
    methods:{
        checkAll(e){
            // this.checkAllTodo(e.target.checked)  //全选或全不选
            this.$emit("checkAllTodo",e.target.checked)
        },
        // 清除所有已经完成的todo
        clearAll(){
            this.$emit("clearAllTodo")
        }
    },
    
}

</script>

<style>
    /* footer */
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
      position:relative;
      top:-1px;
      vertical-align: middle;
      margin-right: 5px;
  }
  .todo-footer button {
      float: right;
      margin-top: 5px;
  }
</style>