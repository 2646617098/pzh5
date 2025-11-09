<template>
    <h1>用户登录</h1>
    <van-form @submit="onSubmit">
        <van-cell-group inset>
            <van-field v-model="form.userName" name="用户名" label="用户名" placeholder="用户名"
                :rules="[{ required: true, message: '请填写用户名' }]" />
            <van-field v-model="form.passWord" type="password" name="密码" label="密码" placeholder="请输入密码"
                :rules="[{ required: true, message: '请填写密码' }]" />
        </van-cell-group>
        <div>
            <van-button class="btn" type="primary" round block native-type="submit">提交</van-button>
        </div>
    </van-form>
</template>
<script setup>
import { reactive, getCurrentInstance } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter()
// 获取当前vue实例
const { proxy } = getCurrentInstance()

// 表单数据
const form = reactive({
    userName: '15668565056',
    passWord: '123456a'
})
// 表达引入
const onSubmit = async () => {
    // 另一种写法，不知道为什么网络显示的是ws请求，但是接受的参数打印是正确的
    const { data } = await proxy.$api.login(form)
    if (data.code === 10000) {
        localStorage.setItem('h5_token', data.data.token)
        localStorage.setItem('h5_userInfo', JSON.stringify(data.data.userInfo))
        router.push('/')
    }
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