<template>
  <div class="app-wrapper">
    <div class="headnav">
      <div class="leftInfo">
        <img class="logo" src="../assets/img/4009378885960.png" alt="">
        <h2 class="title">SAP业务协作平台</h2>
      </div>
      <div class="rightInfo">
        <span class="mr5">{{dateTime}}</span>
        <span class="mr5" @click="login">欢迎<strong>{{username?'【西藏神威药业有限公司】':'【请登录】'}}使用！</strong></span>
        <!-- <span class="mr10">个人信息</span> -->
        <span class="mr10" @click="logout">退出</span>
      </div>
    </div>
    <div class="main-container mans">
      <div>
        <sidebar />
      </div>
      <app-main />
    </div>
  </div>
</template>

<script>
import { Navbar,Sidebar, AppMain } from './components'
// import ResizeMixin from './mixin/ResizeHandler'

export default {
  name: 'Layout',
  components: {
    Navbar,
    Sidebar,
    AppMain
  },
  data(){
    return{
      dateTime:'',
      username:'',
    }
  },
  created(){
    this.dateTime = this.getNowFormatDay();
    this.username = sessionStorage.getItem('zzzloginname')?sessionStorage.getItem('zzzloginname'):''
  },
  methods: {
    login(){
      if(!this.username){
        this.$router.push({
          path:'/login'
        });
      }
    },
    logout() {
      sessionStorage.removeItem('zzzloginname');
      sessionStorage.removeItem('loginperson');
      sessionStorage.removeItem('ldz_admin_token');
      this.$router.push(`/login`)
    },
    getNowFormatDay(nowDate) {
        var char = "-";
        if(nowDate == null){
            nowDate = new Date();
        }
        var day = nowDate.getDate();
        var month = nowDate.getMonth() + 1;//注意月份需要+1
        var year = nowDate.getFullYear();
        //补全0，并拼接
        return year + char + this.completeDate(month) + char +this.completeDate(day);
    },
 
    getNowFormatTime() {
        var nowDate = new Date();
        var colon = ":";
        var h = nowDate.getHours();
        var m = nowDate.getMinutes();
        var s = nowDate.getSeconds();
        //补全0，并拼接
        return this.getNowFormatDay(nowDate) + " " + this.completeDate(h) + colon + this.completeDate(m) + colon + this.completeDate(s);
    },
 
    //补全0
    completeDate(value) {
        return value < 10 ? "0"+value:value;
    }
  }
}
</script>

<style lang="scss" scoped>
  @import "~@/styles/mixin.scss";
  @import "~@/styles/variables.scss";
  .mans{
    padding-top: 60px;
  }
  .app-wrapper {
    @include clearfix;
    position: relative;
    height: 100%;
    width: 100%;
    .headnav{
      position: fixed;
      top:0;
      left: 0;
      width: 100%;
      height: 60px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      background: #465C71;
      z-index: 10;
    }
    .leftInfo{
      display: flex;
      align-items: center;
      .logo{
        display: block;
        width: 200px;
        height: 45px;
      }
      .title{
        color: #fff;
        font-size: 20px;
        line-height: 45px;
        padding-left: 20px;
      }
    }
    .rightInfo{
      color: #fff;
      font-size: 13px;
      .mr5{
        margin-right: 5px;
        cursor:pointer;
      }
      .mr10{
        margin-right: 10px;
        cursor:pointer;
      }
    }
  }
  .drawer-bg {
    background: #000;
    opacity: 0.3;
    width: 100%;
    top: 0;
    height: 100%;
    position: absolute;
    z-index: 999;
  }

  .fixed-header {
    position: fixed;
    top: 0;
    right: 0;
    z-index: 9;
    width: 100%;
    transition: width 0.28s;
  }
</style>
