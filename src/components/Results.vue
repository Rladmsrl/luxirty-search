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
  display: flex !important;
  align-items: center !important;
}

:deep(.gsc-input-box) {
  border-radius: 8px 0 0 8px !important; /* 左侧圆角 */
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.06) !important;
  border: 1px solid #e2e8f0 !important;
  border-right: none !important; /* 移除右侧边框 */
  transition: all 0.3s ease !important;
  height: 40px !important; /* 固定高度 */
  display: flex !important;
  align-items: center !important;
}

:deep(.gsc-input) {
  padding: 0 12px !important;
  font-size: 14px !important;
}

:deep(.gsc-input-box:focus-within) {
  box-shadow: 0 2px 10px rgba(94, 129, 172, 0.2) !important;
  border-color: #5e81ac !important;
}

:deep(.gsc-search-button-v2) {
  border-radius: 0 8px 8px 0 !important; /* 右侧圆角 */
  padding: 0 16px !important;
  margin-left: 0 !important; /* 移除左边距 */
  background-color: #3366cc !important; /* 与链接颜色匹配 */
  border: 1px solid #3366cc !important;
  border-left: none !important; /* 移除左侧边框 */
  height: 40px !important; /* 与输入框相同高度 */
  display: flex !important;
  align-items: center !important;
  justify-content: center !important;
  transition: background-color 0.2s ease !important;
}

:deep(.gsc-search-button-v2:hover) {
  background-color: #1a53ff !important; /* 与链接悬停颜色匹配 */
}

:deep(.gsc-search-button-v2 svg) {
  width: 16px !important;
  height: 16px !important;
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
  color: #3366cc !important; /* 更鲜明的蓝色链接 */
  text-decoration: none !important;
  transition: color 0.2s ease !important;
  font-weight: 500 !important; /* 稍微加粗 */
}

:deep(.gs-title:hover) {
  color: #1a53ff !important; /* 悬停时更深的蓝色 */
  text-decoration: underline !important;
}

:deep(.gs-snippet) {
  line-height: 1.6 !important;
  color: #333333 !important; /* 深灰色用于正文，提高可读性 */
  font-size: 14px !important; /* 适当调整大小 */
}

:deep(.gs-visibleUrl) {
  color: #2e8b57 !important; /* 海绿色用于 URL，更加清晰 */
  font-size: 13px !important;
}

/* 分页控件美化 */
:deep(.gsc-cursor-page) {
  color: #3366cc !important;
  padding: 6px 12px !important;
  border-radius: 4px !important;
  transition: all 0.2s ease !important;
}

:deep(.gsc-cursor-page:hover) {
  background-color: rgba(51, 102, 204, 0.1) !important;
}

:deep(.gsc-cursor-current-page) {
  background-color: #3366cc !important;
  color: white !important;
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
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2) !important;
  }

  :deep(.gsc-input) {
    background-color: transparent !important;
    color: #e5e9f0 !important;
  }

  :deep(.gs-title) {
    color: #88ccff !important; /* 更亮的蓝色在暗色模式下 */
  }

  :deep(.gs-title:hover) {
    color: #aaddff !important; /* 悬停时的亮蓝色 */
  }

  :deep(.gs-snippet) {
    color: #cccccc !important; /* 浅灰色文本，更好的对比度 */
  }

  :deep(.gs-visibleUrl) {
    color: #6bcc8b !important; /* 亮一点的绿色 */
  }
  
  /* 暗黑模式下的分页控件 */
  :deep(.gsc-cursor-page) {
    color: #88ccff !important;
    background-color: rgba(46, 52, 64, 0.8) !important;
  }
  
  :deep(.gsc-cursor-page:hover) {
    background-color: rgba(136, 204, 255, 0.2) !important;
  }
  
  :deep(.gsc-cursor-current-page) {
    background-color: #88ccff !important;
    color: #2e3440 !important; /* 深色背景 */
  }
  
  :deep(.gsc-search-button-v2) {
    background-color: #81a1c1 !important;
  }

  :deep(.gsc-search-button-v2:hover) {
    background-color: #a1c5e7 !important;
  }
  
  :deep(.gsc-input-box:focus-within) {
    border-color: #88ccff !important;
    box-shadow: 0 2px 10px rgba(136, 204, 255, 0.2) !important;
  }
  
  :deep(.gsc-search-button-v2) {
    background-color: #88ccff !important;
    border-color: #88ccff !important;
    color: #2e3440 !important;
  }

  :deep(.gsc-search-button-v2:hover) {
    background-color: #aaddff !important;
    border-color: #aaddff !important;
  }
}
</style>
