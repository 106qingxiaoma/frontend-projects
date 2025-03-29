<template>
    <ul class="todo.main">
        <!-- 为每个item用过渡实现动画效果 -->
        <!-- 当然，实现这个效果也可以在MyItem中实现，但不需要加group -->
        <!-- 当需要为多个元素实现过渡的动画效果要加上group -->
        <transition-group name="demo" appear>
            <!-- 遍历todos数组，要用多少次MyItem；并且要将每一条todoobj对应属性传给MyItem（用props） -->
            <MyItem v-for="todoobj in todos" :key="todoobj.id" :todo="todoobj" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/>
        </transition-group>
    </ul>
</template>

<script>
import MyItem from './MyItem'   //注意路径和前边的不一样
export default {
    name:"MyList",
    components:{
        MyItem
    },
    props:["todos","checkTodo","deleteTodo"]
}
</script>

<style>
/* 进入的起点、离开的终点 */
.demo-enter,.demo-leave-to{
    transform:translateX(-100%);
}
/* 进入的终点、离开的起点 */
.demo-enter-to,.demo-leave{
    transform:translateX(0);
}
/* 进入、离开界面过程中的效果样式 */
.demo-enter-active,.demo-leave-active{
    transition: 0.5s linear;
}

    /* main */
  .todo-main {
      margin-left: 0px;
      border:1px solid #ddd;
      border-radius: 2px;
      padding: 0;
  }
  .todo-empty {
      height: 40px;
      line-height: 40px;
      border:1px solid #ddd;
      border-radius: 2px;
      padding-left: 5px;
      margin-top: 10px;
  }
  
</style>