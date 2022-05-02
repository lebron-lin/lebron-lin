<template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
                <ToDoHeader @add="add" />
                <ToDoList :todoList="todoList" />
                <ToDoFooter v-show="todoList.length > 0" :todoList="todoList" @checkAllTodo="checkAllTodo" @clearCompleted="clearCompleted"/>
            </div>
        </div>
    </div>
</template>

<script>
import pubsub from 'pubsub-js'
import ToDoHeader from './components/ToDoList/ToDoHeader.vue'
import ToDoList from './components/ToDoList/ToDoList.vue'
import ToDoFooter from './components/ToDoList/ToDoFooter.vue'

export default {
    name: 'App',
    components: {
        ToDoHeader,
        ToDoList,
        ToDoFooter
    },
    data() {
        return {
            todoList:JSON.parse(localStorage.getItem("todoList")) || [],
        }
    },
    mounted() {
        this.$bus.$on("checkTodo",this.checkTodo);
        this.pubId = pubsub.subscribe("deleteTodo",this.deleteTodo);
        //this.$bus.$on("deleteTodo",this.deleteTodo);
        this.$bus.$on("updateTodo",this.updateTodo);
    },
    methods: {
        //添加
        add(e){
            this.todoList.unshift(e);
        },
        //复选框
        checkTodo(id){
            this.todoList.forEach((todo) => {
                if(todo.id === id){
                    todo.isdone = !todo.isdone;
                }
            })
        },
        //删除
        deleteTodo(_,id){
            this.todoList = this.todoList.filter(todo => todo.id !== id);
        },
        //编辑
        updateTodo(id,value){
            this.todoList.forEach((todo) => {
                if(todo.id === id){
                    todo.title = value;
                }
            });
        },
        //是否全选
        checkAllTodo(val){
            this.todoList.forEach((todo) => {
                todo.isdone = val;
            });
        },
        //清除已完成任务
        clearCompleted(){
            this.todoList = this.todoList.filter((todo) => {
                return !todo.isdone;
            });
        }
    },
    watch:{
        todoList:{
            deep: true,
            handler(value){
                localStorage.setItem('todoList', JSON.stringify(value));
            }
        }
    },
    beforeDestroy(){
        this.$bus.$off("checkTodo");
        //this.$bus.$off("deleteTodo");
        pubsub.unsubscribe(this.pubId);
        this.$bus.$off("updateTodo");
    }
}
</script>

<style>
    .body {
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

    .btn-edit {
        color: #fff;
        background-color: skyblue;
        border: 1px solid rgba(103, 159, 180, 0.2);
        margin-right: 5px;
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
