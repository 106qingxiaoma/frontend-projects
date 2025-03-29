<template>
    <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add" v-model="content">
        <!-- 1.@keyup.enter="add"绑定一个键盘事件，按enter键添加一个元素，事件名字add -->
        <!-- 2.v-model="content",这里是为了下面获得表单元素，先把content定义为空。也可以用另一种方法：add(event){console.log(event.target.value) -->
    </div>
</template>

<script>
import {nanoid} from "nanoid"   //nanoid引入使用分别引入
// 引入nanoid，nanoid是用来生成唯一id的小型库
export default {
    name:"MyHeader",
    data(){
        return {
            content:""
        }
    },
    methods:{
        add(){
            if(!this.content.trim()){       //空字符串布尔值是false，取反则为true;trim()是去掉前面的空格，这样写更完整。
            return alert("输入不能为空")
        }
        // step1:获取表单元素
        // console.log(this.content)
        // step2:由于todoobj是由id,title,done组成的，所以要将用户输入的内容包装成todoobj
        const todoobj={id:nanoid(),title:this.content,done:false}
        // id:nanoid()的使用方法
        // step3:实现将表单中输入的内容添加到列表中。
            // 这时要将MyList和MyHeader两个组件建立联系，则需要用到他们共同的父亲App组件。
            // todos是共用的，将其放在App组件
            // 将在MyList组件中的todos数据给父亲App,然后App传送数据给MyList组件；(用props，父传子)
            // MyHeader里的todoobj给App,（子传父）,父亲先给儿子一个函数，儿子调用这个函数。
        // this.addTodoObj(todoobj)   //调用
        this.$emit("addTodoObj",todoobj) //触发自定义事件
        //step4:细节进行修改
            // 当添加完元素之后，输入框为空
        this.content=""
            // 写判断语句判断输入框是否为空，空的话按enter键无效(要写在上方才可以)
        }

    },
    props:["addTodoObj"]  //接收数据
}
</script>

<style>
    /* header */
  .todo-header {
      width: 560px;
      height: 28px;
      font-size: 14px;
      border:1px solid #ccc;
      border-radius: 4px;
      padding: 4px 7px;
  }
  .todo-header input:focus{
      outline:none;
      border-color:rgba(82, 168, 236, 0.8);
      box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),0 0 8px rgba(82, 168, 236, 0.6);
  }
</style>