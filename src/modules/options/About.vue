<template>
  <div class="home">
    <h2 class="about title">关于</h2>
    <el-card class="about-card box-card" shadow="hover">
      <div class="text">
        <div class="item">
          <i class="iconfont el-icon-info"/>
          <span class="prefix">下载管理器插件</span>
          <a class="suffix link" href="https://github.com/zishuangzhu/download-manager">Github</a>
        </div>
        <div class="item">
          <i class="iconfont el-icon-star-on"/>
          <span class="prefix">觉得不错，去
            <a class="link" href="https://chrome.google.com/webstore/detail/ofpglhlcdbjdhlacgbljnildhajfmlei">应用商店</a>
            评个分吧
          </span>
        </div>
      </div>
    </el-card>
    <h2 class="version-header title">版本历史</h2>
    <el-card class="version-card box-card" shadow="hover">
      <div slot="header" class="card-header">
        <span class="version">0.5.1</span>
        <span class="date"><i class="el-icon-date"/>2019-12-13</span>
      </div>
      <div class="text">
        <div class="item">1. 下载危险文件时会显示是否确认下载的提示框</div>
        <div class="item">2. 完善中文和英文显示</div>
      </div>
    </el-card>
    <el-card class="version-card box-card" shadow="hover">
      <div slot="header" class="card-header">
        <span class="version">0.4</span>
        <span class="date"><i class="el-icon-date"/>2019-09-30</span>
      </div>
      <div class="text">
        <div class="item">1. 下载文件百分比进度显示</div>
        <div class="item">2. 下载文件可暂停、恢复下载、取消、删除</div>
        <div class="item">3. 搜索所有已经下载的文件</div>
      </div>
    </el-card>
  </div>
</template>

<script>
  export default {
    name: "About",
    props: {
      title: String
    },
  }
</script>

<!--suppress CssUnusedSymbol -->
<style scoped rel="stylesheet/css">
  .home {
    height: 100%;
    width: 100%;
    padding: 20px;
  }

  .title {
    font-size: 15px;
  }

  /* 通用卡片样式 */
  .box-card {
    width: 600px;
    margin-bottom: 36px;
  }
  .box-card .card-header:before,
  .box-card .card-header:after {
    display: table;
    content: "";
  }
  .box-card .card-header:after {
    clear: both
  }
  .box-card >>> .el-card__header {
    padding: 8px 20px;
  }
  .box-card >>> .el-card__body {
    padding: 16px 20px 10px 20px;
  }
  .box-card .card-header {
    display: inline-block;
    line-height: 24px;
  }
  .box-card .text {
    font-size: 14px;
  }
  .box-card .item {
    margin-bottom: 6px;
  }

  /* 关于信息 卡片 */
  .about-card .item .prefix {
    width: 487px;
    font-weight: bold;
    display: inline-block;
  }
  .about-card .item .suffix {
    font-weight: bold;
    display: inline-block;
  }
  .about-card .item .iconfont {
    font-size: 16px;
    margin-right: 12px;
    color: #409EFF;
  }
  .about-card .item .iconfont.el-icon-star-on {
    font-size: 17px!important;
  }
  .about-card .item .link {
    color: #ff8740;
  }

    /* 版本历史信息 卡片 */
  .version-card .version {
    color: #000;
    font-weight: 700;
    font-size: 16px;
    width: 482px;
    display: inline-block;
  }
  .version-card .date {
    display: inline-block;
  }
  .version-card .date .el-icon-date{
    margin-right: 4px;
  }

</style>
