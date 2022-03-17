<template>
  <div class="dashboard-container">
    <h2 class="titles">供应商用户页面</h2>
    <p class="info">列示系统公告、公司通告及站内通知；及时了解待办事项，回复采购、财务等业务人员提出的问题等</p>
    <h3 class="notipes">暂无权限</h3>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'Dashboard',
  computed: {
    ...mapGetters([
      'name'
    ])
  }
}
</script>

<style lang="scss" scoped>
.homePage{
  display: block;
  width: 688px;
  height: 695px;
  margin:50px auto 0;
}
.dashboard {
  &-container {
    margin: 30px;
    .titles{
      font-size: 20px;
      font-weight: bold;
      color: #666;
      margin-top: 10px;
    }
    .info{
      margin:10px 0;
      color: #696969;
      font-size: 14px;
    }
    .notipes{
      margin:150px auto;
      color: #999;
      font-size: 20px;
      text-align: center;
    }
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
