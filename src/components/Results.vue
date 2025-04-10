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
  min-height: 100vh;
  max-width: var(--center-width);
  min-width: 320px;
  box-sizing: border-box;
  margin: 0 auto; /* 居中显示 */
  padding: 0 16px; /* 两侧添加内边距 */
}

.search-container {
  display: flex;
  align-items: center;
  margin-bottom: 24px;
  margin-top: 28px;
  padding: 0 10px;
  min-height: 52px;
  border-radius: 8px;
  transition: all 0.3s ease; /* 添加过渡效果 */
  position: sticky; /* 使搜索框在滚动时固定 */
  top: 0;
  background-color: rgba(255, 255, 255, 0.95); /* 半透明背景 */
  backdrop-filter: blur(5px); /* 毛玻璃效果 */
  z-index: 10;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05); /* 轻微阴影 */
}

.search-title {
  font-size: 24px;
  font-weight: 600;
  color: #3b4252;
  margin-right: 20px;
  white-space: nowrap;
  user-select: none;
  cursor: pointer;
  transition: color 0.2s ease, transform 0.2s ease; /* 添加过渡效果 */
}

.search-title:hover {
  color: #5e81ac; /* 悬停颜色变化 */
  transform: scale(1.05); /* 轻微放大效果 */
}

/* 自定义搜索框样式 */
:deep(.gsc-control-cse) {
  padding: 0 !important;
  border: none !important;
  background-color: transparent !important;
}

:deep(.gsc-search-box) {
  margin-bottom: 0 !important;
}

:deep(.gsc-input-box) {
  border-radius: 6px !important;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08) !important;
  border: 1px solid #e2e8f0 !important;
  transition: all 0.3s ease !important;
}

:deep(.gsc-input-box:focus-within) {
  box-shadow: 0 2px 10px rgba(94, 129, 172, 0.25) !important;
  border-color: #5e81ac !important;
}

:deep(.gsc-search-button-v2) {
  border-radius: 6px !important;
  padding: 8px 16px !important;
  margin-left: 8px !important;
  background-color: #5e81ac !important;
  border: none !important;
  transition: background-color 0.2s ease !important;
}

:deep(.gsc-search-button-v2:hover) {
  background-color: #4c7195 !important;
}

/* 搜索结果区域样式 */
.search-result-zone {
  flex-grow: 1;
  padding: 10px;
  border-radius: 8px;
  background-color: rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(5px);
}

:deep(.gs-title) {
  color: #5e81ac !important;
  text-decoration: none !important;
  transition: color 0.2s ease !important;
}

:deep(.gs-title:hover) {
  color: #4c7195 !important;
  text-decoration: underline !important;
}

:deep(.gs-snippet) {
  line-height: 1.6 !important;
  color: #4c566a !important;
}

:deep(.gs-visibleUrl) {
  color: #a3be8c !important;
}

/* 针对小屏幕的样式 */
@media (max-width: 600px) {
  .search-container {
    flex-direction: column;
    align-items: flex-start;
    margin-bottom: 16px;
    margin-top: 16px;
    padding: 12px;
    width: 100%;
    box-sizing: border-box;
  }

  .search-title {
    font-size: 20px;
    margin-bottom: 12px;
    margin-right: 0;
  }

  :deep(.gsc-search-box) {
    width: 100% !important;
  }

  :deep(.gsc-input-box) {
    width: 100% !important;
  }
}

/* 黑暗模式样式 */
@media (prefers-color-scheme: dark) {
  .search-container {
    background-color: rgba(46, 52, 64, 0.95);
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
  }

  .search-title {
    color: #e5e9f0;
  }

  .search-title:hover {
    color: #88c0d0;
  }

  .search-result-zone {
    background-color: rgba(46, 52, 64, 0.5);
  }

  :deep(.gsc-input-box) {
    background-color: #3b4252 !important;
    border-color: #4c566a !important;
  }

  :deep(.gsc-input) {
    background-color: transparent !important;
    color: #e5e9f0 !important;
  }

  :deep(.gs-title) {
    color: #88c0d0 !important;
  }

  :deep(.gs-title:hover) {
    color: #8fbcbb !important;
  }

  :deep(.gs-snippet) {
    color: #d8dee9 !important;
  }

  :deep(.gs-visibleUrl) {
    color: #a3be8c !important;
  }

  :deep(.gsc-search-button-v2) {
    background-color: #5e81ac !important;
  }

  :deep(.gsc-search-button-v2:hover) {
    background-color: #81a1c1 !important;
  }
}
</style>
