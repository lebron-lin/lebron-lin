<template>
    <div class="todo-footer">
        <label>
            <!-- <input type="checkbox" :checked="isAll" @change="checkAll"> -->
            <input type="checkbox" v-model="isAll">
        </label>
        <span>
            <span>已完成 {{isdonecount}}</span> / 全部 {{total}}
        </span>
        <button class="btn btn-danger" @click="clearCompletedTask">清除已完成任务</button>
    </div>
</template>

<script>
export default {
    name: 'ToDoFooter',
    props: ['todoList'],
    computed: {
        total(){
            return this.todoList.length;
        },
        isdonecount(){
            return this.todoList.reduce((pre,current) => pre + (current.isdone ? 1 : 0), 0);
            //#region 另一种写法
            // var count = 0;
            // this.todoList.forEach((todo) => {
            //     if(todo.isdone){
            //         count++;
            //     }
            // });
            // return count;
            //#endregion
        },
        isAll:{
            get(){
                return this.isdonecount === this.total && this.total > 0
            },
            set(value){
                this.$emit("checkAllTodo",value);
            }
        }
    },
    methods: {
        //全选
        checkAll(e){
            this.$emit("checkAllTodo",e.target.checked);
        },
        //清除已完成任务
        clearCompletedTask(){
            this.$emit("clearCompleted");
        }
    }
}
</script>

<style scoped>
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