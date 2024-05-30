# game_unity_demo
Unity游戏的接入demo源码

> 1. 项目(DanMuClock) 演示了C# Unity游戏接入CPPSDK，与主播端、小程序前端通信及投射图层到主播端的流程
> 2. 若游戏本身不需要中途与小程序主播端、小程序前端通信且也不需要自定义投射图层到主播端，则可以不需要引入该CPPSDK；通过前端小程序调用hyExt.exe.launchGame时传参启动参数，这样启动游戏后，可从启动命令行参数中获取自定义参数即可
> 3. 若游戏仅通信，不投射图层，则参照CLock.cs文件中不调用SendViedeo接口即可，主播端会自动采集游戏画面并添加图层

## 1. 重要文件说明
* MediaPiplelineControllerSDK.cs：C#包装CPP SDK，包括一些初始化或通信细节处理，开发者可不做调整修改直接引入到unity工程中使用即可, 此外注意：CPPSDK(MediaPiplelineControllerSDK.dll相关文件)不需要再打包到游戏包中，因主播端环境已提供
* Clock.cs：示例用例类，其引入MediaPiplelineControllerSDK.cs文件，并解释了初始化、更新游戏画面、通信调用或回调注册、协议包装、回包解析等，具体可以参照该源码实现

## 2. 发布部署
* 如本样例所示，本样例的输出为GameDemo.exe，发布时只需要把GameDemo.exe和游戏资源压缩成zip包(不含目录压缩)，然后在小程序开放平台素材上传zip包获取url地址，最后在小程序里配置url地址
* 构建包可执行文件和依赖库等建议均数字签名，避免被杀毒/防火墙或其他安全软件阻止或误删除文件，导致无法正常启动或运行
* 构建包可执行文件名应与小程序开放平台时部署的应用程序名一致，以避免无法正常启动游戏
* 若需要游戏启动参数，可在小程序开放平台时部署配置，除了业务侧的参数，还有其他额外固定的命令行参数；实际启动时的命令行参数形如：GameDemo.exe --roomid="10009692" --profileid="unkYq1edcPojnerDOCLUZouLPiK8Q7XAVR" --userid="unkYq1edcPojnerDOCLUZouLPiK8Q7XAVR" --foo=bar；其中roomid、profileid、userid为固定命令行参数，foo则为业务侧自定义参数(名称可自定义)

## 3. 其他参考
* [开放平台JS SDK文档](https://dev.huya.com/docs/miniapp/dev/sdk/)
* [弹幕玩法云启动全流程介绍](https://dev.huya.com/docs/miniapp/danmugame/intro/)
* [官方推荐小程序Demo在这里](https://github.com/huya-ext/hyext-examples/tree/master/examples/exe)
* [开发小提示(develop_tip.png)](develop_tip.png)
* [C++ Demo示例及开发流程说明](https://github.com/weigod/game_sdk_demo)

## 4. 开发建议
* 游戏进程启动时，建议优先加载该库，即优先初始化该CPPSDK，初始化后再下载或加载其他游戏资源或初始化游戏相关，此后根据需要再调用平台相关的API，此可避免因初始化后就立即调用平台接口时出现未准备就绪的异常情况
* 若游戏压缩包不是很大，建议整包上传；但游戏包若比较大，建议调整缩减包大小，或者对于部分资源可以在启动游戏时下载，这样打包上传游戏包会小很多，也可避免主播首次启动时长时间等待下载进度
* 若游戏启动时下载游戏资源的场景，建议下载到游戏进程所在的某路径下；下次启动游戏时可检测是否需要下载相应游戏资源包，避免再次重复下载，优化主播使用体验
* 云端模式和本地模式有个区别，若云端模式时，关闭游戏窗口不会触发云端游戏关闭窗口事件(即类似于强制杀死云端进程)，故不要在关闭窗口时做一些退出登录或结算相关等业务操作，避免云端无法执行到，再次启动云端模式时游戏出现其他非预期的行为，如无法正常结算、无法再次登录等
