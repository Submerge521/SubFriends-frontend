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
        <a class="nav-textLeft" @click="toLogin">账号密码登录</a>
        <span></span>
        <a class="nav-textRight">注册</a>
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
      <van-field
          v-model="checkPassword"
          type="password"
          name="checkPassword"
          label="确认密码"
          placeholder="请再次输入密码"
          :rules="[{ required: true, message: '请再次输入密码' }]"
      />
      <van-field name="radio" label="性别">
        <template #input>
          <van-radio-group v-model="gender" direction="horizontal">
            <van-radio name="0">男</van-radio>
            <van-radio name="1">女</van-radio>
          </van-radio-group>
        </template>
      </van-field>
      <van-field
          v-model="phone"
          type="phone"
          name="phone"
          label="手机号"
          placeholder="请输入手机号"
          :rules="[{ required: true, message: '请输入手机号' }]"
      />
      <van-field
          v-model="stuCode"
          type="stuCode"
          name="stuCode"
          label="11位学号"
          placeholder="请输入您的学号"
          :rules="[{ required: true, message: '请输入您的学号' }]"
      />
      <van-field
          v-model="email"
          type="email"
          name="email"
          label="邮箱"
          placeholder="请输入邮箱"
          :rules="[{ required: true, message: '请输入邮箱' }]"
      />
    </van-cell-group>

    <div style="margin: 20px;">
      <van-button round block type="primary" native-type="submit">
        注册
      </van-button>
    </div>
  </van-form>
</template>

<script setup>
import myAxios from "../plugins/myAxios";
// import {showFailToast, showSuccessToast} from "vant";
import {ref} from "vue";
import {useRoute, useRouter} from "vue-router";
import {Toast} from "vant";

const router = useRouter();

const route = useRoute();

/**
 * 登录跳转
 */
const toLogin = () => {
  router.push('/user/login');
}

/**
 * 注册个人信息
 * @type {Ref<UnwrapRef<string>>}
 */
const userAccount = ref('');
const userPassword = ref('');
const checkPassword = ref('');
const gender = ref('0');
const stuCode = ref('');
const phone = ref('');
const email = ref('');

const onSubmit = async () => {
  console.log("用户注册");
  const res = await myAxios.post("/user/register", {
    userAccount: userAccount.value,
    userPassword: userPassword.value,
    checkPassword: checkPassword.value,
    gender: gender.value,
    stuCode: stuCode.value,
    phone: phone.value,
    email: email.value,
  });
  if (res.code === 0 && res.data != null) {
    Toast.success("注册成功");
    await router.push("/user/login");
    // 跳转到之前的页面
    // const redirectUrl = route.query?.redirect?? '/';
    // window.location.href = redirectUrl;
    // router.replace("/")
  } else {
    Toast.fail(res.description)
  }
};

</script>

<style scoped>
.logo{
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 15% 0px 20px 0px;
}
.logo-img{
  margin-right: 0px;
}
.logo-title{
  position: relative;
  color: rgba(0,0,0,.85);
  font-weight: 600;
  font-size: 30px;
}
.bars-nav{
  width: 80%;
  height: 50px;
  margin: 5px auto;
  border-bottom: 1px solid #e8eaed;
}
.nav-text{
  height: 50px;
  display: flex;
  justify-content: center;
}
.nav-text span{
  width: 30px;
}
.nav-text .nav-textLeft{
  color: #1a1a1a;
  text-shadow: 0 0 .25px currentcolor;
  line-height: 50px;

}
.nav-text .nav-textRight{
  color: #1890ff;
  text-shadow: 0 0 .25px currentcolor;
  line-height: 50px;
  border-bottom: 3px solid #1890ff;
}
</style>
