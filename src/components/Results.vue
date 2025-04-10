<template>
  <div class="my-container">
    <div class="search-container" id="searchContainer">
      <h1 class="search-title" @click="goHome">Search</h1>
      <div class="gcse-searchbox"></div>
    </div>
    <div class="search-result-zone">
      <div class="gcse-searchresults" data-linkTarget="_blank" data-refinementStyle="link"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SearchPage',
  props: ['query'],
  mounted() {
    this.loadGoogleCSE();
    this.setupResultsRenderedCallback();  // 注册渲染结果回调函数
    if (!this.query) {
      this.goHome();
    }
  },
  methods: {
    loadGoogleCSE() {
      const script = document.createElement('script');
      script.src = `https://cse.google.com/cse.js?cx=${import.meta.env.VITE_GOOGLE_CSE_CX}`;
      script.async = true;
      document.head.appendChild(script);
    },
    setTitle() {
      var inputContent = document.getElementsByName('search')[0].value;
      document.title = inputContent + ' - Luxirty Search'
    },
    goHome() {
      // 使用 window.location.href 跳转到根路径
      window.location.href = '/';
    },
    setupResultsRenderedCallback() {
      // 定义一个渲染回调函数，用于移除不需要的属性
      const myWebResultsRenderedCallback = () => {
        const links = document.querySelectorAll('a.gs-title');

        links.forEach((anchor) => {
          // 移除 'data-cturl' 和 'data-ctorig' 属性
          anchor.removeAttribute('data-cturl');
          anchor.removeAttribute('data-ctorig');
        });

        // 设置搜索标题，多页签时更好切换
        this.setTitle();
      };

      // 将回调注册到 Google Custom Search 引擎对象
      window.__gcse || (window.__gcse = {});
      window.__gcse.searchCallbacks = {
        web: {
          rendered: myWebResultsRenderedCallback,
        },
      };
    }
  }
};
</script>
<style scoped>
.my-container {
  display: flex;
  flex-direction: column;
  /* 让子元素垂直排列 */
  min-height: 100vh;
  /* 让容器占满整个视窗高度 */
  max-width: var(--center-width);
  min-width: 320px;
  box-sizing: border-box;
}

.search-container {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
  margin-top: 26px;
  margin-left: 28px;
  min-height: 48px;
}

.search-title {
  font-size: 24px;
  color: #58636f;
  margin-right: 20px;
  white-space: nowrap;
  user-select: none;
  /* 防止标题文字被选中 */
}

/* 针对小屏幕的样式 */
@media (max-width: 600px) {
  .search-container {
    flex-direction: column;
    align-items: center;
    margin-bottom: 10px;
    margin-top: 10px;
    margin-left: 0px;
    min-height: 48px;
    width: 100vw;
  }

  .search-title {
    font-size: 20px;
    margin-bottom: 10px;
    margin-right: 0;
  }
}

/* 黑暗模式样式 */
@media (prefers-color-scheme: dark) {
  .search-title {
    color: #d1d5db;
    /* 黑暗模式下的颜色 */
  }
}

.search-result-zone {
  flex-grow: 1;
  /* 让搜索结果区占据剩余空间 */
}
</style>
