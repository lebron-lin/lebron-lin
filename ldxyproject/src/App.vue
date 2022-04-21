<template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
                <ToDoHeader :add="add" />
                <ToDoList :todoList="todoList" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/>
                <ToDoFooter />
            </div>
        </div>
    </div>
</template>

<script>
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
            todoList:[
                {id:'0001',title:'抽烟',isdone:true},
                {id:'0002',title:'喝酒',isdone:false},
                {id:'0003',title:'烫头',isdone:true}
            ]
        }
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
        deleteTodo(id){
            this.todoList = this.todoList.filter(todo => todo.id !== id);
        }
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
