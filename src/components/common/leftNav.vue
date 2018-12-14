<template>
   <div class="left-nav">
       <ul>
           <li><i class="icon iconfont icon-goumai"></i><div>收银</div> </li>
           <li><i class="icon iconfont icon-dianpu"></i><div>店铺</div> </li>
           <li><i class="icon iconfont icon-hanbao"></i><div>商品</div> </li>
           <li><i class="icon iconfont icon-huiyuanqia"></i><div>会员</div> </li>
       </ul>
   </div>
</template>
<script>
export default {
    
}
</script>

<style>
    .left-nav{
        width: 5%;
        background-color:blue;
        float: left;
        height: 100%;
        font-size: 10px;
        color: aliceblue
    }
    ul{
        margin: 0;
        padding: 0;
    }
    ul li{
        list-style: none;
        text-align: center;
        border-bottom: 1px solid aliceblue;
        padding: 10px;
    }
    ul li:hover{
        background-color: orange;
        cursor: pointer;
    }
</style>
