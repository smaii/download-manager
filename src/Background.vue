<template>
  <div id="app"></div>
</template>

<!--suppress JSUnresolvedVariable, JSUnresolvedFunction, UnterminatedStatementJS -->
<script>
/* eslint-disable no-undef */
import storage from "./utils/storage"
export default {
  name: 'app',
  // 插件初始化配置
  mounted () {
    storage.defaultSettings()

    // 取消下载时浏览器下方出现的下载信息按钮
    this.disableDownloadBottom()

    // 在文件下载开始时添加监听器
    chrome.downloads.onCreated.addListener(() => {
      this.downloadProgress()
    })
  },
  data () {
    return {
      anyInProgress: false,
      tid: -1,
      downloadingNumber: 0,
      dangerousDownloadingNumber: 0,
      downloadMessage: {
        type: 'download',
        data: []
      }
    }
  },
  watch: {
    // 只有当下载文件数量有改动时才重新设置图标badge
    downloadingNumber (val) {
      this.setBrowserBadge(val)
    },
    dangerousDownloadingNumber (val) {
      if (val > 0) {
        // 在下载危险文件时，将图标正在下载中的数字背景设置为红色
        chrome.browserAction.setBadgeBackgroundColor({color: '#FF0000'})
      } else {
        // 确认下载危险文件时，将图标正在下载中的数字背景重新设置为蓝色
        chrome.browserAction.setBadgeBackgroundColor({color: '#4285F4'})
      }
    }
  },
  methods: {
    // 禁用每次下载时页面浏览器下方的下载进度提示
    disableDownloadBottom() {
      if (chrome.downloads.setShelfEnabled) {
        chrome.downloads.setShelfEnabled(false)
      }
    },

    // 文件下载进度

    downloadProgress() {
      this.tid = -1
      this.anyInProgress = false
      chrome.downloads.search({orderBy: ['-startTime']}, (items) => {
        let downloadingNumber = 0
        let dangerousDownloadingNumber = 0
        items.forEach((item) => {
          if (item.state === 'in_progress') {
            downloadingNumber++
            this.anyInProgress = true

            if ((item.danger !== 'safe') && (item.danger !== 'accepted')) {
              dangerousDownloadingNumber++
            }
          }
        })

        // icon右小角显示正在下载中的文件数量
        this.downloadingNumber = downloadingNumber

        this.dangerousDownloadingNumber = dangerousDownloadingNumber

        // 使用vue.set更新数据
        this.$set(this.downloadMessage, 'data', items)

        // 发送数据到popup
        chrome.runtime.sendMessage(JSON.stringify(this.downloadMessage))

        if (this.anyInProgress && this.tid < 0) {
          while (this.tid < 0) {
            this.tid = setTimeout(this.downloadProgress, 200)
          }
        }
      })
    },

    // 设置图标右上角显示的正在下载中文件的数量
    setBrowserBadge(number) {
      let text = ''
      if (number > 0) {
        if (number >= 100) {
          text = '99+'
        } else {
          text = number.toString()
        }
      }
      chrome.browserAction.setBadgeText({text: text})
    }
  }
}
</script>
