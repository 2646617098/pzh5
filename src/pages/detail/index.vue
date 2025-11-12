<template>
    <div class="container">
        <div class="header">
            <van-icon @click="goBack" name="arrow-left" class="header-left" size="30" />
            填写服务信息
        </div>
        <statusBar :item="stateMap[detailData.trade_state]" />
        <div class="tips">
            <div class="dzf" v-if="detailData.trade_state === '待支付'">
                <div class="text1">订单待支付</div>
                <div class="text2">请在
                    <counter :second="second" />内完成支付，超时自动取消
                </div>
                <div class="text3">
                    <van-button type="success" @click="showCode = true">立即支付0.5元</van-button>
                </div>
            </div>
        </div>
        <van-dialog v-model:show="showCode" :show-confirm-button="false">
            <van-icon name="cross" class="close" @click="closeCode" />
            <div>微信支付</div>
            <van-image :src="codeImg" width="150" height="150" style="margin:10px" />
            <div>请使用微信扫码支付</div>
        </van-dialog>
    </div>
</template>
<script setup>
import { useRoute, useRouter } from 'vue-router';
import statusBar from '../../components/statusBar.vue'
import { computed, getCurrentInstance, onMounted, reactive, ref } from 'vue';
import counter from '../../components/counter.vue'
import Qrcode from 'qrcode';

const router = useRouter()
const route = useRoute()

const { proxy } = getCurrentInstance()

// 详情页数据
const detailData = reactive({})

// 计算倒计时
const second = computed(() => {
    return detailData.order_start_time ? detailData.order_start_time + 7200000 - Date.now() : 0
})

// 支付弹窗
const showCode = ref(false)
const codeImg = ref()
const closeCode = () => {
    showCode.value = false
}
const stateMap = {
    '待支付': 10,
    '待服务': 20,
    '已完成': 30,
    '已取消': 40
}

onMounted(async () => {
    const { data } = await proxy.$api.orderDetail({ oid: route.query.oid })
    Object.assign(detailData, data.data)
    // console.log(detailData, "detailData")

    Qrcode.toDataURL(orderRes.data.wx_code).then((url) => {
        codeImg.value = url
        showCode.value = true
    })
})


// 返回
const goBack = () => {
    router.go(-1)
}
</script>
<style lang="less" scoped>
.container {
    background-color: #f0f0f0;
    height: 100vh;
}

.header {
    background-color: #fff;
    line-height: 40px;
    text-align: center;

    .header-left {
        float: left;
    }
}

.card {
    margin: 15px 0;
    padding: 10px;

    .header-text {
        padding-left: 5px;
        line-height: 30px;
        font-size: 16px;
        font-weight: bold;
        border-left: 4px solid red;
    }
}

.dzf {
    padding: 20px;

    .text1 {
        font-size: 20px;
        font-weight: bold;
        line-height: 30px;
        color: #666;
    }

    .text2 {
        font-size: 14px;
        color: #666;
    }

    .text3 {
        text-align: center;

        .van-button {
            margin-top: 10px;
            margin-left: 10px;
            width: 80%;
            font-weight: bold;
        }
    }
}

::v-deep(.van-dialog__content) {
    text-align: center;
    padding: 20px;

    .close {
        position: absolute;
        left: 20px;
    }
}
</style>