# game_unity_demo
Unity游戏的接入demo源码

> 1. 项目(DanMuClock) 演示了C# Unity游戏接入CPPSDK，与主播端、小程序前端通信及投射图层到主播端的流程
> 2. 若游戏本身不需要中途与小程序主播端、小程序前端通信且也不需要自定义投射图层到主播端，则可以不需要引入该CPPSDK；通过前端小程序调用hyExt.exe.launchGame时传参启动参数，这样启动游戏后，可从启动命令行参数中获取自定义参数即可
> 3. 若游戏仅通信，不投射图层，则参照CLock.cs文件中不调用SendViedeo接口即可，主播端会自动采集游戏画面并添加图层

## 1. 重要文件说明
* MediaPiplelineControllerSDK.cs：C#包装CPP SDK，包括一些初始化或通信细节处理，开发者可不做调整修改直接引入到unity工程中使用即可
* Clock.cs：示例用例类，其引入MediaPiplelineControllerSDK.cs文件，并解释了初始化、更新游戏画面、通信调用或回调注册、协议包装、回包解析等，具体可以参照该源码实现

## 2. 发布部署
* 如本样例所示，本样例的输出为GameDemo.exe，发布时只需要把GameDemo.exe和游戏资源压缩成zip包(不含目录压缩)，然后在小程序开放平台素材上传zip包获取url地址，最后在小程序里配置url地址

## 3. 其他参考
* [开放平台JS SDK文档](https://dev.huya.com/docs/miniapp/dev/sdk/)
* [弹幕玩法云启动全流程介绍](https://dev.huya.com/docs/miniapp/danmugame/intro/)
* [官方推荐小程序Demo在这里](https://github.com/huya-ext/hyext-examples/tree/master/examples/exe)
* [开发小提示(develop_tip.png)](develop_tip.png)
* [C++ Demo示例及开发流程说明](https://github.com/weigod/game_sdk_demo)

