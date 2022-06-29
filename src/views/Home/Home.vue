<template>
  <div class="home-container">
    <van-nav-bar title="头条" :fixed="true" />
    <!-- 导入、注册，并使用ArticleInfo组件 -->
    <van-pull-refresh
      v-model="isLoading"
      @refresh="onRefresh"
      :disabled="finished"
    >
      <van-list
        v-model="loading"
        :finished="finished"
        finished-text="没有更多了"
        @load="onLoad"
      >
        <ArticleInfo
          v-for="item in artlist"
          :key="item.id"
          :title="item.title"
          :author="item.aut_name"
          :cmtCount="item.comm_count"
          :time="item.pubdate"
          :cover="item.cover"
        ></ArticleInfo>
      </van-list>
    </van-pull-refresh>
  </div>
</template>

<script>
// 按需导入API接口
import { getArticleListAPI } from '@/api/articleAPI'

import ArticleInfo from '@/components/Article/ArticleInfo.vue'

export default {
  name: 'Home',
  data() {
    return {
      // 页码值
      page: 1,
      // 每页显示多少条数据
      limit: 10,
      // 文章的数组
      artlist: [],
      // 是否正在加载下一页数据，如果loading为true，则不会反复触发load事件
      // 加载完下一页数据请求回来后，要把它改为false
      loading: true,
      // 所有数据是否加载完毕了，如果没有更多数据了，一定要把它改为true
      finished: false,
      // 是否正在下拉刷新
      isLoading: false,
    }
  },
  created() {
    this.initArticleList()
  },
  methods: {
    // 封装获取文章列表数据的方法
    async initArticleList(isRefresh) {
      // 发起GET请求,获取文章的列表数据
      const { data: res } = await getArticleListAPI(this.page, this.limit)
      if (isRefresh) {
        // 证明是下拉刷新，新数据在前，后数据在后
        // this.artlist = [新数据在前，后数据在后]
        this.artlist = [...res, ...this.artlist]
        this.isLoading = false
      } else {
        // 如果是上拉加载更多，那么应该是
        // this.artlist = [旧数据在前，新数据在后]
        this.artlist = [...this.artlist, ...res]
        // this.artlist = res
        this.loading = false
      }

      if (res.length === 0) {
        // 证明没有下一页
        this.finished = true
      }
    },
    onLoad() {
      // console.log('触发了load事件')
      // 1.让页码值+1
      this.page++
      // 2.重新请求接口数据
      this.initArticleList()
    },
    onRefresh() {
      // console.log('触发了下拉')
      // 1.让页码值+1
      this.page++
      // 2.重新请求接口数据
      this.initArticleList(true)
    },
  },

  components: { ArticleInfo },
}
</script>

<style lang="less" scoped>
.home-container {
  padding: 46px 0 50px;
}
.van-nav-bar {
  background-color: #007bff;
}
/deep/.van-nav-bar__title {
  //注意加/deep/
  color: white;
}
</style>
