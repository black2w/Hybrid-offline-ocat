<template>
  <div>
    <el-row>
        <el-col :span="9"><div class="placeholder_hiden">1</div></el-col>
        <el-col :span="6">
            <div style="margin-top: 100px;">
                <h3 style="text-align:center;">移动平台 Beta 0.0.1</h3>
                <el-form :rules="rules" ref="ruleForm" :model="ruleForm">
                    <el-form-item prop="username">
                        <el-input prefix-icon='el-icon-user' v-model="ruleForm.username" placeholder="请输入用户名" clearable></el-input>
                    </el-form-item>
                    <el-form-item prop="password">
                        <el-input prefix-icon='el-icon-view' v-model="ruleForm.password" placeholder="请输入密码" type="password" @change="loginAction('ruleForm')" clearable></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="loginAction('ruleForm')" size="medium" style="width: 100%">登录</el-button>
                    </el-form-item>
                  <el-form-item>
                    <el-button type="primary" @click="openAPPAction('ruleForm')" size="medium" style="width: 100%">测试打开APP</el-button>
                  </el-form-item>
                </el-form>
            </div>
        </el-col>
        <el-col :span="9"><div class="placeholder_hiden">1</div></el-col>
    </el-row>
  </div>
</template>

<script>

import CallApp from "callapp-lib";

export default {
  data () {
    return {
      ruleForm: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      }
    }
  },
  mounted () {
    if (localStorage.username) {
      this.ruleForm.username = localStorage.username
    }
  },
  methods: {
    loginAction (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.loginApi()
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    openAPPAction (formName) { //打开app的方法
      window.location.href = "https://cloud.heyshare.cn/app/open";

      // const options = {
      //   scheme: {
      //     protocol: 'heyshare2://'//APP 协议，URL Scheme 的 scheme 字段，就是你要打开的 APP 的标识
      //   },
      //   appstore: "https://apps.apple.com/cn/app/1566825253" ,//填写appstore的下载地址
      //   yingyongbao: '',//填写应用宝的下载地址
      //   fallback: '',//填写唤端失败后跳转的地址
      //   //小技巧哈！要是仿照支付宝这种检测没有app后，点击打开app留在当前页面，appstore、yingyongbao、
      //   //fallback都写当前地址就好了window.location.href获取一下就好了
      //   timeout: 500,
      // };
      // const callLib = new CallApp(options);
      //
      // callLib.open({
      //   param: '',
      //   path: ''
      // });
      // //小技巧哈！这里因为后台直接返回没有Scheme的路径部分所以路由信息和参数是在
      // //一起我就懒得处理了就直接给到path也可以正常执行的比如：
      // // Scheme:///aaaa/bbbb?login=1  整体路径
      // // /aaaa/bbbb?login=1  path部分直接赋值上去就不用在处理参数问题了
      // console.log(callLib.generateScheme({
      //   //打印最终路径。。。。
      // }));

      // var d = new Date();
      // var t0 = d.getTime();
      //
      // if (openApp('heyshare2://')) {
      //   openApp('heyshare2://')
      // } else {
      //   var delay = setInterval(function(){
      //     var d = new Date();
      //     var t1 = d.getTime();
      //     if( t1-t0<3000 && t1-t0>2000){
      //       alert('请下载APP');
      //       window.location.href = "https://apps.apple.com/cn/app/1566825253";
      //     }
      //     if(t1-t0>=3000){
      //       clearInterval(delay);
      //     }
      //   },1000);
      // }


      // const t = 1000;
      // const t1 = Date.now();
      // const ifr = document.createElement('iframe');
      // // 下面的this.url 是需要跳转到app的schema链接
      // ifr.src = 'heyshare2://';
      // ifr.setAttribute('src', 'heyshare2://');
      // ifr.setAttribute('style', 'display:none');
      // document.body.appendChild(ifr);
      // // window.location.href = 'heyshare2://';
      // setTimeout(function() {
      //   // 启动app时间较长处理
      //   const t2 = Date.now();
      //   document.body.removeChild(ifr)
      //   if (t2 - t1 < t + 100) {
      //     console.log('hasApp', false);
      //     // 没有安装App，跳转到对应的App下载页面。
      //     // 下面的例子App下载页面也是H5页面。
      //     // window.location.href = "https://apps.apple.com/cn/app/1566825253";
      //     window.location.href = "https://apps.apple.com/cn/app/6447845953";
      //   } else {
      //     console.log('hasApp', true)
      //     // 已经安装App，下面的this.url是需要跳转到app的schema链接。
      //     window.location.href = 'heyshare2://';
      //   }
      // }, t);




      // if (navigator.userAgent.match(/(iPhone|iPod|iPad);?/i)) {
      //   window.location = ('heyshare2://');//schema链接或者universal link
      //   // window.setTimeout(() => { //如果没有安装app,便会执行setTimeout跳转下载页
      //   //   window.location.href = "https://apps.apple.com/cn/app/1566825253"; //ios下载地址3
      //   // }, 3000);
      //
      //   var ifr = document.createElement("iframe");
      //   ifr.src = "heyshare2://"; /***打开app的协议，ios同事提供***/
      //   ifr.style.display = "none";
      //   document.body.appendChild(ifr);
      //   window.setTimeout(function(){
      //     document.body.removeChild(ifr);
      //     window.location.href = "https://apps.apple.com/cn/app/1566825253";
      //   },2000);
      // }
    },
    openApp(src){
    // 通过iframe的方式试图打开APP，如果能正常打开，会直接切换到APP，并自动阻止a标签的默认行为
      // 否则打开a标签的href链接
      var ifr = document.createElement('iframe');
      ifr.src = src;
      ifr.style.display = 'none';
      document.body.appendChild(ifr);
      window.setTimeout(function(){
      document.body.removeChild(ifr);
    },2000);
},
    loginApi: async function () {
      let params = this.ruleForm
      const res = await this.$http.post(this.$http.API.LOGIN, params)
      if (res.success === true) {
        if (res.data.enabled === 2) {
          this.$message({
            message: '用户被锁定',
            type: 'warning'
          })
        } else {
          this.$message({
            message: '登录成功啦',
            type: 'success'
          })
          localStorage.username = this.ruleForm.username
          this.$store.commit('setUserInfo', res.data)
          console.log('localstore username:' + localStorage.username)
          console.log('查看的登录信息：' + this.$store.getters.loginInfo)
          console.log('是否登录：' + this.$store.getters.isLogin)
          this.$menuPermission.checkPermissions()
          console.log('是否存在离线包管理这个菜单1：'+this.$store.getters.hasButtonById('12'))
          console.log('是否存在离线包管理这个菜单2：'+this.$menuPermission.HAS_USER_CREATE_BUTTON)
          this.$router.push('/home')
        }
      }
    }
  }
}
</script>

<style scoped>
.placeholder_hiden {
  visibility: hidden;
}
</style>
