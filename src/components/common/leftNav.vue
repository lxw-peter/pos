<template>
    <div class="leftNav">
        <ul>
            <li><i class="icon iconfont icon-renwuguanli"></i><div>收银</div></li>
            <li><i class="icon iconfont icon-jinrudianpu"></i><div>店铺</div></li>
            <li><i class="icon iconfont icon-liebiao1"></i><div>商品</div></li>
            <li><i class="icon iconfont icon-31huiyuanqia"></i><div>会员</div></li>
            <li><i class="icon iconfont icon-quanbudingdan"></i><div>统计</div></li>
        </ul>
    </div>
</template>
<script>
export default {
    name: 'leftNav',
    data(){
        return {

        }
    },
  
}
</script>

<style lang='less' scoped>
// 1rem = 16px
.leftNav {
    color: #fff;
    font-size: 1rem;
    height: 100%;
    background-color: rgb(9, 143, 196);
    float: left;
    width: 5%;
    overflow: hidden;
    li {
        text-align: center;
        border-bottom: 1px solid #20a0ff;
        padding: 0.6rem;
    }
    .iconfont {
        font-size: 1.5rem;
        margin-left: -0.2rem;
        margin-bottom: 0.3rem;
        display: inline-block;
    }

}


</style>
