<template>
  <div class="app-wrapper">
    <div class="headnav">
      <div class="leftInfo">
        <img class="logo" src="@/assets/img/4009378885960.png" alt="">
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
      <div>
        <div class="login-container">
          <h2 class="titles">登录</h2>
          <p class="info">  请输入登录账号和密码；如果您遗忘或丢失了帐户或密码，请与管理人员联系。如果您有意成为我们的供应商</p>
          <fieldset>
            <legend>账户信息</legend>
            <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form loginfr" auto-complete="on" label-position="left">
              <el-form-item prop="loginname">
                <label class="mr10 pname">登录账号</label>
                <el-input
                  ref="loginname"
                  v-model="loginForm.loginname"
                  placeholder="请输入企业账号"
                  name="loginname"
                  type="text"
                  tabindex="1"
                  auto-complete="on"
                />
              </el-form-item>
              <el-form-item prop="password" >
                <label class="mr10 pname">登录密码</label>
                <el-input
                  :key="passwordType"
                  ref="password"
                  v-model="loginForm.password"
                  :type="passwordType"
                  placeholder="请输入密码"
                  name="password"
                  tabindex="2"
                  auto-complete="on"
                  @keyup.enter.native="handleLogin"
                />
              </el-form-item>
              <el-form-item prop="password">
                <label class="mr10 pname">用户类型</label>
                <el-select v-model="loginForm.loginperson" placeholder="请选择">
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-button :loading="loading" type="primary" style="width:80px;margin-bottom:30px;" @click.native.prevent="handleLogin">登录</el-button>

              <!-- <div class="tips">
                <span style="margin-right:20px;">loginname: admin</span>
                <span> password: any</span>
              </div> -->

            </el-form>
          </fieldset>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Navbar,Sidebar } from '../../layout/components'
// import ResizeMixin from './mixin/ResizeHandler'
import { setToken } from '@/utils/auth'
export default {
  name: 'Layout',
  components: {
    Navbar,
    Sidebar
  
  },
  data(){
    const validateUsername = (rule, value, callback) => {
      callback()
    }
    const validateLoginperson = (rule, value, callback) => {
      callback()
    }
    
    const validatePassword = (rule, value, callback) => {
      if (value.length < 4) {
        callback(new Error('密码输入错误'))
      } else {
        callback()
      }
    }
    return{
      
      dateTime:'',
      username:'',
      value:'1',
      loginForm: {
        loginname: '',
        password: '',
        loginperson:'1',
      },
      loginRules: {
        loginname: [{ required: true, trigger: 'blur', validator: validateUsername }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }],
      },
      options: [{
          value: '1',
          label: '供应商'
        },{
          value: '2',
          label: '企业用户'
        },
      ],
      loading: false,
      passwordType: 'password',
      redirect: undefined
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
      this.$router.push({
        path:'/login'
      });
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
    },
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          if(this.loginForm.loginname == "1085"){
            if((this.loginForm.loginperson == '1' && this.loginForm.password =="sw123") || this.loginForm.loginperson == '2' && this.loginForm.password =="sw+123"){
              this.loading = false
              setToken('1111');
              sessionStorage.setItem('zzzloginname',this.loginForm.loginname);
              sessionStorage.setItem('loginperson',this.loginForm.loginperson);
              this.$router.push({ path: this.redirect || '/' })
              
            }else{
              this.loading = false
              this.$message({
                message: '个人账号或密码错误',
                type: 'error'
              });
            }
            
          }else{
            this.loading = false
            this.$message({
              message: '企业账号错误',
              type: 'error'
            });
          }
          
        } else {
          console.log('error submit!!')
          return false
        }
      })
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
.pname{
  display: inline-block;
  color: #333;
  font-size: 12px;
}
.mr10{
  margin-right: 10px;
}
fieldset{
  width: 100%;
  height: auto;
  padding: 12px 0 0 30px;
  border: 1px solid #ccc;
}
legend{
  padding: 2px 4px 8px 4px;
  font-size: 14px;
}
.login-container{
  padding: 20px;
}
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

</style>

<style lang="scss" scoped>
.el-input{
  display: inline-block!important;
  width: 200px!important;
}
.el-input__inner{
  display: inline-block!important;
  width: 200px!important;
}
</style>
