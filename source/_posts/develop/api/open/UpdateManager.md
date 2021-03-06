---
title: UpdateManager
header: develop
nav: api
sidebar: UpdateManager
---





**解释**：管理更新，[swan.getUpdateManager](https://smartprogram.baidu.com/docs/develop/api/get/)返回值。

**百度APP中扫码体验：**

<img src="https://b.bdstatic.com/miniapp/assets/images/doc_demo/pages_getUpdateManager.png"  class="demo-qrcode-image" />

**方法参数**：无

**代码示例**

<a href="swanide://fragment/a215f5f8430d830160fc485621797da81575376239973" title="在开发者工具中预览效果" target="_self">在开发者工具中预览效果</a>

* 在 swan 文件中

```html
<view class="container">
    <view class="card-area">
        <view class="top-description border-bottom">是否下载最新版本</view>
        <button type="primary" bindtap="applyUpdate">button</button>   
    </view>
</view>
```


```js
Page({
    onLoad(){
        const updateManager = swan.getUpdateManager();
        this.updateManager =  updateManager;
    },
    applyUpdate() {
        this.updateManager.onCheckForUpdate(function (res) {
            // 请求完新版本信息的回调
            console.log("res", res.hasUpdate);
            if(!res.hasUpdate){
                swan.showModal({
                    title: '更新提示',
                    content: '无可用更新版本',
                });
            }
            else {
                this.updateManager.onUpdateReady(function (res) {  
                    swan.showModal({
                        title: '更新提示',
                        content: '新版本已经准备好，是否重启应用？',
                        success(res) {
                            if (res.confirm) {
                                // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
                                updateManager.applyUpdate();
                            }
                        }
                    });
                });
                this.updateManager.onUpdateFailed(function (err) {
                    // 新的版本下载失败
                    swan.showModal({
                        title: '更新提示',
                        content: '新版本下载失败，请稍后再试'
                    });
                });
            }
        }); 
    }
});
```
