<template>
    <div>
        <input :value="username" type="text" lable="请输入用户名"/><br/><br/>
        <input :value="userpwd" type="password" lable="请输入密码"/><br/><br/>
        <button @click="login">登录</button>
    </div>
</template>

<script>
export default {
    name: "NewLogin",
    data(){
        return {
            username: "",
            userpwd: "",
        }
    },
    methods: {
        login(){
            console.log(this);
            console.log(666 + "username：" + this.username + "userpwd：" + this.userpwd);
        }
    },
}
</script>

<style>

</style>