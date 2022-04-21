<template>
    <li>
        <label>
            <input type="checkbox" :checked="todoItem.isdone" @change="handlecheck(todoItem.id)">
            <span>{{todoItem.title}}</span>
        </label>
        <button class="btn btn-danger" @click="deleteTodoItem(todoItem.id)">删除</button>
    </li>
</template>

<script>
export default {
    name: 'ToDoItem',
    //接受父组件得传值
    props: ['todoItem','checkTodo','deleteTodo'],
    methods: {
        handlecheck(id){
            this.checkTodo(id);
        },
        deleteTodoItem(id){
            if(confirm('确定删除嘛?')){
                this.deleteTodo(id);
            }
        }
    },
}
</script>

<style scoped>
    li {
        list-style: none;
        height: 36px;
        line-height: 36px;
        padding: 0 5px;
        border-bottom: 1px solid #ddd;
    }

    li label {
        float: left;
        cursor: pointer;
    }

    li label li input {
        vertical-align: middle;
        margin-right: 6px;
        position: relative;
        top: -1px;
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

    li:hover {
        background-color: gray;
    }

    li:hover button {
        display: block;
    }
</style>