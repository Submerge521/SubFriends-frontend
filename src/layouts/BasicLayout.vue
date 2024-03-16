<template>
  <van-sticky>
    <van-nav-bar
        v-if="!$route.meta.showNav"
        :title="title"
        left-arrow
        @click-left="onClickLeft"
        @click-right="onClickRight"
    >
      <template #right>
        <van-icon name="search" size="18" />
      </template>
    </van-nav-bar>
  </van-sticky>

  <div id="content">
    <router-view />
  </div>

  <van-tabbar route @change="onChange"  v-if="!$route.meta.showNav">
    <van-tabbar-item to="/" icon="home-o" name="index">主页</van-tabbar-item>
    <van-tabbar-item to="/team" icon="search" name="team">队伍</van-tabbar-item>
    <van-tabbar-item to="/user" icon="friends-o" name="user">个人</van-tabbar-item>
  </van-tabbar>
</template>

<script setup lang="ts">
import {useRouter} from "vue-router";
import {ref} from "vue";

import routes from "../config/route";
import {Toast} from "vant";

const router = useRouter();
const DEFAULT_TITLE = 'Submerge-伙伴速配';
const title = ref(DEFAULT_TITLE);

/**
 * 根据路由切换标题
 */
router.beforeEach((to, from) => {
  const toPath = to.path;
  const route = routes.find((route) => {
    return toPath == route.path;
  })
  title.value = route?.title ?? DEFAULT_TITLE;
})

const onClickLeft = () => {
  router.back()
};
const onClickRight = () => (
    // 字符串路径
    router.push('/search')
);

const active = ref('index');
const onChange = (index: any) => Toast(`标签 ${index}`);
</script>

<style scoped>
#content{
  padding-bottom: 50px;
}
</style>
