<template>
  <div>
    <!-- 占位符 -->
    <router-view></router-view>
    <!-- 底部区域 -->
    <van-tabbar route>
      <van-tabbar-item replace to="/home" icon="home-o">首页</van-tabbar-item>
      <van-tabbar-item replace to="/user" icon="user-o">我的</van-tabbar-item>
    </van-tabbar>
  </div>
</template>

<script>
export default {
  name: 'App',
}
</script>

<style></style>
