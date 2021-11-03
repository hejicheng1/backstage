<template>
  <div class="login">
    <div class="login_contain">
      <!-- 头像区域 -->
      <img src="../assets/img/bao.png" alt="" />
      <!-- 登录表单区域 -->
      <!-- 登录 -->
      <div class="login_froms">
        <el-form
          labal-width="0px"
          :model="login_from"
          ref="loginFormRef"
          :rules="loginFormRules"
        >
          <el-form-item prop="username">
            <el-input
              prefix-icon="iconfont icon-user"
              v-model="login_from.username"
            ></el-input>
          </el-form-item>
          <!-- 密码 -->
          <el-form-item prop="password">
            <el-input
              prefix-icon="iconfont icon-3702mima"
              type="password"
              v-model="login_from.password"
            ></el-input>
          </el-form-item>
        </el-form>
      </div>
      <!-- 按钮 -->
      <el-form>
        <el-form-item class="btn">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetloginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "login",
  data() {
    return {
      login_from: {
        username: "admin",
        password: "123456",
      },
      // 验证表单的验证规则对象
      loginFormRules: {
        username: [
          { required: true, message: "请输入登录名称", trigger: "blur" },
          {
            min: 3,
            max: 10,
            message: "账号长度在 3 到 10 个字符",
            trigger: "blur",
          },
        ],
        password: [
          { required: true, message: "请输入登录密码", trigger: "blur" },
          {
            min: 6,
            max: 15,
            message: "密码长度在 6 到 15 个字符",
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
    resetloginForm() {
      this.$refs.loginFormRef.resetFields();
      // console.log(this);
    },
    login() {
      this.$refs.loginFormRef.validate(async (valid) => {
        // console.log(valid);
        if (!valid) return;
        const { data: res } = await this.$http.post("login", this.login_from);
        // console.log(res);
        if (res.meta.status !== 200) return this.$message.error("登录失败!");
        // console.log("登录成功");
        this.$message.success("登录成功!");
        // 1.将登陆成功之后的 token 保存到客户端的 sessionStorage中
        // 2. 1.1项目中除了登录之外的其他API接口,必须在登录之后才能访问
        //     1.2token只在当前网站打开期间生效,所以将token保存在ssessionStorage中
        window.sessionStorage.setItem("token", res.data.token);
        // 2.通过编程式导航跳转到后台主页 ,路由地址是 /home
        this.$router.push("/home");
      });
    },
  },
};
</script>
<style>
.login {
  height: 100%;
  background-color: #2b4b6b;
  position: relative;
}
.login_contain {
  margin: auto;
  width: 450px;
  height: 300px;
  background-color: #fff;
  /* 固定定位也可以居中 */
  /* position: fixed;
  left: 40%;
  top: 30%; */
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.login_contain img {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
  left: 50%;
  padding: 10px;
  box-shadow: 0 0 20px black;
}
.btn {
  display: flex;
  justify-content: flex-end;
  position: absolute;
  right: 20px;
  bottom: 0;
}
.login_froms {
  position: absolute;
  bottom: 50px;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
</style>
