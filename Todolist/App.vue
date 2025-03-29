<template>
  <div class="root">
        <div class="todo-container">
            <div class="todo-wrap">
                <MyHeader @addTodoObj="addTodoObj"/>   <!-- 将addTodoObj传给MyHeader组件 --><!-- 完善：把：变为@，变成自定义事件 -->
                <!-- <MyList :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/>   将todos传给MyList组件 -->
                <!-- 将勾选和删除使用全局事件总线，不通过MyList传递，直接通过App给MyItem -->
                <MyList :todos="todos"/> 
                <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
            </div>
        </div>
    </div>
</template>

<script>
import MyHeader from './components/MyHeader.vue'
import MyList from './components/MyList.vue'
import MyFooter from './components/MyFooter.vue'
export default {
    name: 'App',
    data(){
        return {
            // todos:[
            //     {id:"001",title:"打代码",done:"true"},  //id一般用字符串，done表示完成
            //     {id:"002",title:"睡觉",done:"false"},
            //     {id:"003",title:"吃饭",done:"true"}
            // ]
            // 读取数据
            todos:JSON.parse(localStorage.getItem("todos")) || []
            // 如果不加" || []",当用户什么也不输入，todos取空数组，而不会报错（其他组件用到todos可能会报错）
        }
    },
    // 进行本地存储，使用户可以在刷新页面的基础上保留其添加删除的数据
    // 首先对数据进行保存,那么要监视todos是否发生变化，发生变化就用新的todos取代
    watch:{
        todos:{
            deep:true,    //深度监视，当刷新时勾选的内容不会消失
            handler(newvalue){   //handle(newvalue,oldvalue)新的todos和旧的todos
                localStorage.setItem("todos",JSON.stringify(newvalue))
            }
        }
    },
    components: {MyHeader,MyList,MyFooter},
    methods:{
        // 添加
        addTodoObj(todoobj){
            this.todos.unshift(todoobj)
        },
        // 勾选
        checkTodo(id){
            this.todos.forEach((todo)=>{
                if(todo.id===id) todo.done=!todo.done
            })
        },
        //1-7编辑-将表单中的内容显示到item中
        updateTodo(id,title){
            this.todos.forEach((todo)=>{
                if(todo.id===id) todo.title=title
            })
        },
        // 删除
        // deleteTodo(id){
        //     this.todos.forEach((todoobj,index) =>{
        //         if(todoobj.id===id){
        //             this.todos.splice(index,1,id)
        //         }
        //     })
        // },
        // 或者：
        deleteTodo(id){
            this.todos=this.todos.filter(todo=>todo.id!==id
        )},
        // 全选或全不选
        checkAllTodo(done){
                this.todos.forEach(todo=>todo.done=done)
        },
        // 清除所有已经完成的todo
        clearAllTodo(){
            this.todos=this.todos.filter((todo)=>!todo.done)
        }
    },
    // 进行全局事件总线，首先确定是MyItem传给App,则在App中绑定事件和回调函数,然后在MyItem中触发时间
    mounted(){
        this.$bus.$on("gouxuan",this.checkTodo),
        this.$bus.$on("shanchu",this.deleteTodo),
        // 1-6为bus绑定一个事件(使用事件总线)
        this.$bus.$on("bianji",this.updateTodo)
    },
    beforeDestroy(){
        // 解绑
        this.$bus.$off("gouxuan"),
        this.$bus.$off("shanchu"),
        // 1-7 解绑
        this.$bus.$off("bianji")
    }
}
</script>
<style>
  /* base */
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
      cursor:pointer;
      box-shadow: inset 0 1px 0 rgba(255, 255,255, 0.2),0 1px 2px rgba(0, 0, 0, 0.05);
      border-radius: 4px;
  }
  .btn-danger {
      color:#fff;
      background-color: #da4f49;
      border:1px solid #bd362f;
  }
  .btn-edit {
    color: #fff;
    background-color: skyblue;
    border:1px solid rgb(103, 159, 180);
    margin-right: 5px;
  }
  .btn-danger:hover {
      color:#fff;
      background-color: #bd362f;
  }
  .btn:focus {
      outline:none;
  }
  .todo-container {
      width: 600px;
      margin: 0 auto;
  }
  .todo-container .todo-wrap {
      padding: 10px;
      border:1px solid #ddd;
      border-radius: 5px;
  }
</style>
