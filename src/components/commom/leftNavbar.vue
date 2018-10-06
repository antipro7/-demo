<template>
    <div class="left-navbar">
        <ul>
            <li>
                <i class="icon iconfont icon-liebiao"></i>
                <div>目录</div>
            </li>
            <li>
                <i class="icon iconfont icon-dianpu"></i>
                <div>店铺</div>
            </li>
            <li>
                <i class="icon iconfont icon-hanbao"></i>
                <div>商品</div>
            </li>
            <li>
                <i class="icon iconfont icon-huiyuanqia"></i>
                <div>会员</div>
            </li>
            <li>
                <i class="icon iconfont icon-goumai"></i>
                <div>支付</div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    
}
</script>

<style>
html,body{
    height: 100%;
}

.left-navbar{
    color: #fff;
    height: 100%;
    font-size: 10px;
    background-color: #1d8ce0;
    float: left;
    width: 5%;
}

.iconfont{
    font-size: 24px;
}

.left-navbar ul{
    padding: 0;
    margin: 0;
}

.left-navbar li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding: 6px;
}
</style>
