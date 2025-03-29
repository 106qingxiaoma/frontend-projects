<template>
    <li>
        <label>
            <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)">
            <!--1. :checked="todo.done"根据done是true还是false确定是否默认勾选 -->
            <!-- @click="handleCheck"注册点击事件确认是否勾选 -->
            <!-- <input type="checkbox" :checked="todo.done" v-model="todo.done">
            这样写可以直接实现，但是不推荐，因为props里的数据最好不要修改，这样的话虽然不会报错，但是修改了props里todo的数据。 -->
            <!-- 2.插值语法，对应需要的值 -->
            <!-- 1-3 通过isEdit属性来确定是否显示输入的值(表单显示，值不显示)-->
            <span v-show="!todo.isEdit">{{todo.title}}</span>
            <!-- 1-3通过isEdit属性来确定是否显示表单 -->
            <!-- 1-4给表单绑定一个失去焦点的事件blur,注意传递的参数-->
            <input type="text" :value="todo.title" v-show="todo.isEdit" @blur="handleBlur(todo,$event)" ref="inputTitle">
        </label>
        <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
        <!-- 新要求：实现删除前边有个编辑按钮，点击按钮可以出现一个表单，在表单中输入内容，当表单失去焦点时，表单中的内容替代原本内容 -->
        <!-- 1-1首先给编辑按钮在App中设定样式（通用样式）；然后绑定一个事件，当点击它时表单出现 -->
        <button class="btn btn-edit" @click="handleEdit(todo)">编辑</button>
    </li>
</template>

<script>
export default {
    name:"MyItem",
    // 1.接收来自MyList组件传来的信息
    props:["todo","checkTodo","deleteTodo"],
    methods:{
        handleCheck(id){
            // 找到勾选项对应的id
            // 在todos找到对应的id
            // 给对应id的done值取反
            // 调用App中的函数
            // this.checkTodo(id)  
            // 使用全局总线 
            this.$bus.$emit("gouxuan",id)
        },
        handleDelete(id){
            if (confirm("确定删除吗")) {
                // this.deleteTodo(id)
                // 使用全局总线 
            this.$bus.$emit("shanchu",id)
            }
        },
        //编辑
        handleEdit(todo) {
            // 1-2在todos中增加一个isEdit属性，值为布尔值；当值为true，显示表单
            // 注意：将isEdit值设为true，需要满足响应式
            this.$set(todo,"isEdit",true)
            // 需要实现点击编辑时出现表单的同时自动获取焦点
            // 1.使用this.$refs.inputTitle.focus()不能达到目的，
            // 原因是：会把这两行代码执行完后解析模板，这样又会失去焦点。
            //2.使用定时器可以实现：
            // setTimeout(() => {
            //     this.$refs.inputTitle.focus()
            // });
            // 这种方法是利用了异步事件，原理是等执行完同步事件再去执行异步事件。
            // 3.使用API:$nextTick实现（比较推荐）
            this.$nextTick(function(){
                this.$refs.inputTitle.focus()
            })
            // 语法：this.$nextTick(回调函数)
            // 原理：当在它上边的代码执行完毕去解析完模板之后再回来执行this.$nextTick里的函数。
            // 场景：当已改变数据后，基于新的DOM进行某些操作时，在$nextTick回调函数中执行。
        },
        // 1-5注意：这里必须要传todo，因为需要todo里的属性isEdit和id
        handleBlur(todo,event){
            // 1-6失去焦点需要满足两件事：表单消失和表单中编辑的文字显现在item中
            // 表单消失需要把isEdit值设为false
            this.$set(todo,"isEdit",false)
            // 表单中编辑的文字显现在item中  要实现和App组件之间的通信，将数据传给App(使用事件总线)
            // 告诉他修改的id是多少，修改的title是什么，把这两个要素都传过去
            // event.target.value可以获得表单的值
            this.$bus.$emit("bianji",todo.id,event.target.value)
        }
    }
}

</script>

<style scoped>
    /* item */
  li {
      list-style: none;
      height: 36px;
      line-height: 36px;
      padding: 0 5px;
      border-bottom: 1px solid #ddd;
  }
  li label {
      float: left;
      cursor:pointer;
  }
  li label li input {
      vertical-align: middle;
      margin-right: 6px;
      position: relative;
      top:-1px;
  }
  li button {
      float: right;
      display: none;
      margin-top: 3px;
  }
  li:before {
      content: initial;
  }
  li:last-child {
      border-bottom: none;
  }
  /* 后添加的 */
  li:hover {
    background-color: #ddd;
  }
  li:hover button {
    display: block;
  }
  
</style>