
### 配置flutter全局环境

#### 安装flutter SDK

- 下载flutter SDK。https://docs.flutter.dev/get-started/install/macos#get-sdk
- 在~（用户）目录下新建development目录
- 将解压文件目录flutter整个拖拽进dev目录
- 打开~下的.zshrc 或 bashrc 或 .bash_profile 
- 输入  `export PATH="$PATH:/Users/han/SDK/flutter/bin/"`并保存
- 更新文件 source .zshrc （如果不生效，关闭terminal重新打开）
- 输入flutter，命令生效

#### IOS环境配置

```bash
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -runFirstLaunch
```

根据提示，按一次回车，然后按住空格看证书，翻到底部，再输入'agree'。

用Flutter构建ios应用的环境已经搭建好。

可以试着键入命令`open -a Simulator`，打开一个ioc模拟器。

#### Android环境配置
https://www.bilibili.com/video/BV1M94y1f79j?p=6 这个教程后半部分
- 下载 Android studio
- 一路默认到底
- 打开软件，选择VSD manager 配置模拟器
- 创建一个新模拟器，安装pixel 3a 模拟器
- 在新窗口中点击下载安卓12进行下载，完成后就可以使用安卓模拟器

#####  检测flutter环境

运行`flutter doctor` 查看返回结果.


错误一：**Run `path/to/sdkmanager --install "cmdline-tools;latest"`**
解决方案：https://blog.csdn.net/qq_25482087/article/details/121785594

错误二：cocoapods未安装
解决方案：sudo gem install cocoapods

错误三：## HTTP Host Availability

1.  打开`/path-to-flutter-sdk/packages/flutter_tools/lib/src/http_host_validator.dart`文件，修改`https://maven.google.com/`为 google maven 的国内镜像，如`https://maven.aliyun.com/repository/google/`
2.  删除`/path-to-flutter-sdk/bin/cache` 文件夹
3.  重新执行`flutter doctor`

或者使用https://github.com/flutter/flutter/issues/98170中提到的方案，复制ss的复制终端代理命令

### vscode调试flutter

- 终端调试
	- 打开安卓模拟器
	- 在vscode终端运行flutter run
	- 在终端按下R，是热重载，按下shift + R是重新载入热重载。
- vscode调试
	- 顶部菜单栏，启用调试
	- 选择flutter & dart即可开启自动热重载，保存代码模拟器自动刷新
	- ⚡是热重载，刷新是重新刷新代码进入热重载

