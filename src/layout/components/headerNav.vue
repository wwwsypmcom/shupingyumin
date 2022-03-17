<template>
  <div>
    <el-menu
      v-if="username"
      :default-active="activeIndex2"
      router
      class="el-menu-demo"
      mode="horizontal"
      background-color="#98A6BC"
      text-color="#000"
      active-text-color="#FF9900">
      
      <el-menu-item index="1">首页</el-menu-item>
      <el-submenu index="2">
        <template slot="title">交流平台</template>
        <el-menu-item index="2-1" :route = "{'path':'/platform/index'}">交流信息查询</el-menu-item>
      </el-submenu>
      <el-submenu index="3">
        <template slot="title">采购订单协同</template>
        <el-menu-item index="3-1" :route = "{'path':'/platform/order'}">未送货订单查询</el-menu-item>
      </el-submenu>
      <el-submenu index="4">
        <template slot="title">采购入库</template>
        <el-menu-item index="4-1" :route = "{'path':'/platform/inshop'}">商品入退库查询</el-menu-item>
        <el-menu-item index="4-2" :route = "{'path':'/platform/outshop'}">商品未开票查询</el-menu-item>
      </el-submenu>
      <el-submenu index="5">
        <template slot="title">财务对账</template>
        <el-menu-item index="5-1" :route = "{'path':'/platform/jiesuan'}">结账单查询</el-menu-item>
      </el-submenu>
      <el-submenu index="6">
        <template slot="title">商品查询</template>
        <el-menu-item index="6-1" :route = "{'path':'/platform/shoppingxskc'}" >商品库存查询</el-menu-item>
        <el-menu-item index="6-2" :route = "{'path':'/platform/shoppingxs'}">商品销售查询</el-menu-item>
        <el-menu-item index="6-3" :route = "{'path':'/shopping/index'}">商品配送查询</el-menu-item>
      </el-submenu>
      <el-submenu index="7">
        <template slot="title">商品比价</template>
        <el-menu-item index="7-1" :route = "{'path':'/platform/price'}">商品比价模块</el-menu-item>
      </el-submenu>
    </el-menu>
    <el-menu
      v-if="!username"
      :default-active="activeIndex2"
      @select="handleSelect"
      router
      class="el-menu-demo"
      mode="horizontal"
      background-color="#98A6BC"
      text-color="#000"
      active-text-color="#FF9900">
      
      <el-menu-item index="1" :route = "{'path':'/platform/jianjie?name=漱玉平民简介'}">漱玉平民简介</el-menu-item>
      <el-menu-item index="2" :route = "{'path':'/platform/zuji?name=漱玉平民足迹'}">漱玉平民足迹</el-menu-item>
    </el-menu>
  </div>
  

</template>
<script>
export default {
  data(){
    return{
      activeIndex2:'',
      username:'',
    }
  },
  created(){
    this.username = sessionStorage.getItem('zzzloginname')?sessionStorage.getItem('zzzloginname'):''
  },
  methods:{
    handleSelect(val){
      this.activeIndex2 = val
    }
  }
}
</script>
<style lang = "scss">
.el-menu-demo{
  height: 36px;
  .el-menu-item{
    height: 35px;
    line-height: 35px;
    font-size: 14px;
  }
  .el-submenu{
    padding: 0 20px;
    border-left: 1px solid #FF9900;
    border-right: 1px solid #FF9900;
  }
  
}
.el-menu-demo.el-menu--horizontal>.el-submenu .el-submenu__title{
  height: 35px!important;
  line-height: 35px!important;
}
</style>
