<template>
    <h1>用户登录</h1>
    <van-form @submit="onSubmit">
        <van-cell-group inset>
            <van-field v-model="userName" name="用户名" label="用户名" placeholder="用户名"
                :rules="[{ required: true, message: '请填写用户名' }]" />
            <van-field v-model="passWord" type="password" name="密码" label="密码" placeholder="请输入密码"
                :rules="[{ required: true, message: '请填写密码' }]" />
        </van-cell-group>
    </van-form>
    <div>
        <van-button class="btn" type="primary" round block native-type="submit">提交</van-button>
    </div>
</template>
<script setup>
import { reactive } from 'vue';

// 表单数据
const form = reactive({
    userName: '',
    passWord: ''
})
// 表达引入
const onSubmit = () => {

}
</script>
<style lang="less" scoped>
h1 {
    text-align: center;
}

.btn {
    margin: 16px;
}
</style>