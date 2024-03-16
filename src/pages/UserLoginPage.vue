<template>
  <van-form @submit="onSubmit">
    <div class="logo">
      <span class="logo-img">
       <van-image
           width="44px"
           height="44px"
           position="right"
           src="../src/images/Friends.png"
       />
      </span>
      <span class="logo-title">Submerge-伙伴速配</span>
    </div>

    <div class="bars-nav">
      <div class="nav-text">
        <a class="nav-textLeft">账号密码登录</a>
        <span></span>
        <a class="nav-textRight" @click="toRegister">注册</a>
      </div>
    </div>

    <van-cell-group inset>
      <van-field
          v-model="userAccount"
          name="userAccount"
          label="用户名"
          placeholder="请输入用户名"
          :rules="[{ required: true, message: '请填写用户名' }]"
      />
      <van-field
          v-model="userPassword"
          type="password"
          name="userPassword"
          label="密码"
          placeholder="请输入密码"
          :rules="[{ required: true, message: '请填写密码' }]"
      />
    </van-cell-group>

            <div class="item">
<!--              <div class="register">-->
<!--                <a @click="toRegister">没有账号？去注册</a>-->
<!--              </div>-->
<!--              <div class="interval">-->
<!--                <span style="line-height: 40px">|</span>-->
<!--              </div>-->
              <div class="forget">
                <a @click="toSubmerge">忘记密码？</a>
              </div>
            </div>

<!--    <van-button type="primary" @click="toRegister">立即注册</van-button>-->
    <div style="margin: 20px;">
      <van-button round block type="primary" native-type="submit">
        登录
      </van-button>
    </div>
  </van-form>
</template>

<script setup>
import myAxios from "../plugins/myAxios";
import {ref} from "vue";
import {useRoute, useRouter} from "vue-router";
import {Toast} from "vant";

const router = useRouter();

const route = useRoute();

const userAccount = ref('');
const userPassword = ref('');

const onSubmit = async () => {
  console.log("用户登录");
  const res = await myAxios.post("/user/login", {
    userAccount: userAccount.value,
    userPassword: userPassword.value
  });
  console.log("res.code= " + res.code)
  console.log("res= " + res.data != null)
  if (res.code === 0 && res.data != null) {
    console.log("登录成功")
    Toast.success("登录成功");
    // 跳转到之前的页面
    window.location.href = route.query?.redirect ?? '/index';
    // router.replace("/")
  } else {
    Toast.fail(res.description)
  }
};

/**
 * 注册跳转
 */
const toRegister = () => {
  router.push('/user/register');
}

const toSubmerge = () => {
  window.location.href = 'https://github.com/submerge521';
}
</script>

<style scoped>
.logo {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 15% 0px 20px 0px;
}

.logo-img {
  margin-right: 0px;
}

.logo-title {
  position: relative;
  color: rgba(0, 0, 0, .85);
  font-weight: 600;
  font-size: 30px;
}

.bars-nav {
  width: 80%;
  height: 50px;
  margin: 5px auto;
  border-bottom: 1px solid #e8eaed;
}

.nav-text {
  height: 50px;
  display: flex;
  justify-content: center;
}

.nav-text span {
  width: 30px;
}

.nav-text .nav-textLeft {
  color: #1890ff;
  text-shadow: 0 0 .25px currentcolor;
  line-height: 50px;
  border-bottom: 3px solid #1890ff;
}

.nav-text .nav-textRight {
  color: #1a1a1a;
  text-shadow: 0 0 .25px currentcolor;
  line-height: 50px;
}

.item {
  width: 100%;
  height: 40px;
  display: flex;
  justify-content: right;
  margin-top: 20px;
}

.item .register {
  margin-right: 10px;
}

.item .interval {
  width: 10px;
}

.item .forget {
  margin-left: 10px;
}

.item a {
  line-height: 40px;
  color: #1890ff;
}
</style>
