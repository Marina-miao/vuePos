<template>
  <div id="left">
    <ul class="navList">
        <li>
            <i class="iconfont icon-shouyin"></i>
            <p>收银</p>
        </li>
        <li>
            <i class="iconfont icon-msnui-store"></i>
            <p>店铺</p>
        </li>
        <li>
            <i class="iconfont icon-shangpin-tianchong"></i>
            <p>商品</p>
        </li>
        <li>
            <i class="iconfont icon-huiyuan"></i>
            <p>会员</p>
        </li>
        <li>
            <i class="iconfont icon-tongji"></i>
            <p>统计</p>
        </li>
        <li>
            <i class="iconfont icon-shezhi"></i>
            <p>设置</p>
        </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'navBox',
  data () {
    return {
      
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.iconfont{
    font-size:20px;
}

#left{
    float:left;
    width:8%;
    height:100%;
    background:#1d8ce0;
    color:#fff;
}
#left ul{
    width:100%;
}
#left ul li{
    width:100%;
    margin-top:30px;
}
</style>
