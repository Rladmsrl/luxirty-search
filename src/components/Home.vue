<template>
  <div class="home">
    <div class="logo">
      Search
    </div>
    <div class="search-container">
      <div class="gcse-searchbox-only" data-resultsUrl="search"></div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    const script = document.createElement('script');
    script.src = `https://cse.google.com/cse.js?cx=${import.meta.env.VITE_GOOGLE_CSE_CX}`;
    script.async = true;
    document.body.appendChild(script);
  }
};
</script>

<style scoped>
.home {
  position: relative;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.logo {
  position: absolute;
  top: 35vh; /* 从 30vh 增加到 35vh */
  font-size: 48px;
  font-weight: bold;
}

.search-container {
  position: absolute;
  top: calc(35vh + 100px); /* 相应地调整，保持与 logo 的相对位置 */
  width: 100%;
  display: flex;
  justify-content: center;
}

/* 针对小屏幕的样式 */
@media (max-width: 600px) {
  .logo {
    font-size: 35px;
    top: 37vh; /* 从 30vh 增加到 35vh */
  }
  .search-container {
    top: calc(37vh + 80px);
  }
}


</style>
