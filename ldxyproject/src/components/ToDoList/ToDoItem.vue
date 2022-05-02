<template>
    <li>
        <label>
            <input type="checkbox" :checked="todoItem.isdone" @change="handlecheck(todoItem.id)">
            <span v-show="!todoItem.isEdit">{{todoItem.title}}</span>
            <input ref="inputTitle" type="text" v-show="todoItem.isEdit" :value="todoItem.title" @blur="handleBlur(todoItem,$event)">
        </label>
        <button class="btn btn-danger" @click="deleteTodoItem(todoItem.id)">删除</button>
        <button v-show="!todoItem.isEdit" class="btn btn-edit" @click="editTodoItem(todoItem)">编辑</button>
    </li>
</template>

<script>
import pubsub from 'pubsub-js'
export default {
    name: 'ToDoItem',
    //接受父组件得传值
    props: ['todoItem'],
    methods: {
        handlecheck(id){
            this.$bus.$emit("checkTodo",id);
        },
        //编辑
        editTodoItem(todoItem){
            if(Object.prototype.hasOwnProperty.call(todoItem, "isEdit")){
                todoItem.isEdit = true;
            }else{
                this.$set(todoItem,"isEdit",true);
            }
            this.$nextTick(function(){
                this.$refs.inputTitle.focus();
            })
        },
        //失去焦点
        handleBlur(todoItem,e){
            todoItem.isEdit = false;
            if(!e.target.value.trim()){
                alert("输入不能为空!")
                return;
            }
            this.$bus.$emit("updateTodo",todoItem.id,e.target.value);
        },
        //删除
        deleteTodoItem(id){
            if(confirm('确定删除嘛?')){
                pubsub.publish("deleteTodo",id);
                //this.$bus.$emit("deleteTodo",id);
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