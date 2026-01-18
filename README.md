# Play! 模拟器 #

邻家小熊修改自用~

Play! 是一款适用于 Windows、macOS、UNIX、Android、iOS 及网页浏览器平台的 PlayStation 2（PS2）模拟器。

兼容性相关信息可查阅官方的「兼容性追踪器」：[Compatibility Tracker](https://github.com/jpd002/Play-Compatibility)。
如果某款游戏无法在该模拟器中运行，请在该仓库提交新的 Issue。

如需了解更多信息，可访问官网：[purei.org](https://purei.org)。

你可以体验实验性的网页版模拟器：[playjs.purei.org](https://playjs.purei.org)。

如需交流讨论，欢迎加入我们的 Discord 社群：https://discord.gg/HygQnQP。

## 命令行选项（Windows/macOS/Linux） ##
支持以下命令行选项：
- `--disc "光盘镜像路径"`：启动指定的光盘镜像文件。
- `--elf "ELF 文件路径"`：启动指定的 ELF 可执行文件。
- `--arcade "街机游戏ID"`：启动指定的街机游戏。
- `--state "存档槽位编号"`：从指定槽位加载存档。
- `--fullscreen`：以全屏模式启动模拟器。

## Windows 系统运行说明 ##
请确保你的设备安装了最新版 VC++ 可再发行组件包。如需安装，可参考以下链接：
- x86-64 架构：https://aka.ms/vs/17/release/vc_redist.x64.exe
- x86-32 架构：https://aka.ms/vs/17/release/vc_redist.x86.exe

## iOS 系统运行说明 ##
该模拟器通过 JIT（即时编译）生成代码来提升运行速度，但 iOS 系统默认不支持 JIT，因此有以下额外要求：

- 设备需运行 iOS 13 及以下版本，或搭载 arm64e 芯片的设备运行 iOS 14.2/14.3 版本。
- 设备需完成越狱。

若不满足上述要求，仍可通过其他方式启用 JIT。以下指南说明了启用 JIT 的方法：

https://spidy123222.github.io/iOS-Debugging-JIT-Guides/

Play! 支持通过 AltServer 自动激活 JIT（需 AltServer 与 iOS 设备处于同一网络），你可在模拟器的设置菜单中开启该功能。

你也可以自行编译模拟器，并通过 Xcode 调试器启动以启用 JIT（需安装 iOS SDK）。若不想一直连接 Xcode，可在「调试」面板中点击「Detach（分离）」按钮，分离后调试进程会保留至应用退出。

**注意：若未启用 JIT 就运行游戏，启动时会触发崩溃。**

### 向游戏库添加游戏 ###
如需在 iOS 设备的 Play! 应用中添加光盘镜像，可参考以下官方指南：
- https://support.apple.com/HT201301
- https://support.apple.com/HT210598

若你的设备已越狱，模拟器会自动扫描 `/private/var/mobile` 目录及其所有子目录中的可启动文件。

## 南梦宫 System 2x6 街机游戏支持 ##

### 放置加密狗镜像与光盘镜像 ###
运行街机游戏所需的文件需放在「Play! 数据文件」目录下的 `arcaderoms` 子目录中，目录结构示例：

```
arcaderoms/
  bldyr3b.zip
  bldyr3b/
    bldyr3b.chd  # 游戏光盘镜像
  tekken4.zip
  tekken4/
    tef1dvd0.chd  
```

## 街机专属控制映射 ##
部分街机专属操作映射至 PS2 手柄的以下按键：

- 服务/投币：SELECT 键
- 测试：同时按下 L3 与 R3 键

### 光枪支持 ###
对于支持光枪的游戏，按键映射如下：

- 扳机：圆圈（CIRCLE）键
- 踏板：三角（TRIANGLE）键

模拟器窗口中的鼠标光标位置对应光枪瞄准位置，你也可在控制器设置中将鼠标按键映射为圆圈或三角键，提升操作体验。

**《化解危机 3》特别说明**：该游戏需先在服务菜单中校准光枪。按住测试键进入「I/O Test（输入输出测试）」，再进入「Gun Initialize（光枪初始化）」，按下踏板键校准（瞄准屏幕中心射击），仅需校准一次。

### 太鼓达人支持 ###
对于《太鼓达人》系列游戏，按键映射如下：

- 左侧面（面）：L1 键
- 左侧边（ふち）：L2 键
- 右侧面（面）：R1 键
- 右侧边（ふち）：R2 键

### 赛车游戏支持 ###
对于赛车类游戏，按键映射如下：

- 方向盘：左摇杆 X 轴（左右）
- 油门踏板：左摇杆 Y 轴（上）
- 刹车踏板：右摇杆 X 轴（右）

## 常见问题排查 ##

#### 无法打开 CHD 文件 ####
市面上很多镜像文件格式不规范（尤其是 CD/DVD 游戏镜像），模拟器要求 CDVD 镜像需通过 chdman 工具的 `createcd` 或 `createdvd` 命令压缩。格式不符会触发报错。可通过游戏对应的 `arcadedef` 文件中的 `cdvd` 或 `hdd` 配置项，判断该游戏使用的是 CDVD 还是 HDD 镜像。

你可通过 `chdman` 验证镜像是否为标准 CDVD 格式，执行以下命令：

```
chdman info -i image.chd
```

若输出中包含 `CHT2` 元数据，说明该镜像为 CDVD 格式；
若包含 `GDDD` 元数据，说明该镜像为 HDD 格式，需转换为 CDVD 格式。

HDD 转 CDVD 格式的命令：

```
mv image.chd image.chd.orig
chdman extracthd -i image.chd.orig -o image.iso
chdman createcd -i image.iso -o image.chd
```

## 编译构建 ##

### 前期准备 ###
首先需克隆本仓库（包含模拟器源码及编译所需的子模块）：
 ```
 git clone --recurse-submodules https://github.com/jpd002/Play-.git
 cd Play-
 ```

### Windows 系统编译 ###
Windows 下最简便的编译方式是打开 Qt Creator，选择项目目录下的 `Play-/CMakeLists.txt` 文件。
你也可通过 Visual Studio 或命令行编译，步骤如下：

编译前需安装 CMake：
 ```cmd
 mkdir build
 cd build
 ```
 ```
 # 不指定 -G 会默认生成 32 位项目
 cmake .. -G "Visual Studio 15 2017 Win64" -DCMAKE_PREFIX_PATH="C:\Qt\5.10.1\msvc2017_64" -DUSE_QT=YES
 ```
生成项目后，可打开 Visual Studio 解决方案编译，或继续通过命令行编译：
 ```cmd
 cmake --build . --config Release
 ```
注：`--config` 可选值为 `Release`（发布版）、`Debug`（调试版）、`RelWithDebInfo`（带调试信息的发布版）。

### macOS & iOS 系统编译 ###
若未安装 CMake，可通过 [Homebrew](https://brew.sh) 安装：
 ```bash
 brew install cmake
 ```

macOS 下有两种编译方式：Makefile 或 Xcode：
 ```bash
 mkdir build
 cd build
 ```
 ```
 # 不指定 -G 会默认使用 Makefiles
 cmake .. -G Xcode -DCMAKE_PREFIX_PATH=~/Qt/5.1.0/clang_64/
 cmake --build . --config Release
 # 或使用 Makefiles
 cmake .. -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_PREFIX_PATH=~/Qt/5.1.0/clang_64/
 cmake --build .
 ```
iOS 编译需在 CMake 命令中添加以下参数：
 ```bash
 -DCMAKE_TOOLCHAIN_FILE=../../../Dependencies/cmake-ios/ios.cmake -DTARGET_IOS=ON
 ```

iOS 编译不依赖 Qt，需移除 `-DCMAKE_PREFIX_PATH=...` 参数。

示例：
 ```bash
 cmake .. -G Xcode -DCMAKE_TOOLCHAIN_FILE=../deps/Dependencies/cmake-ios/ios.cmake -DTARGET_IOS=ON
 ```

注：通过 Makefiles 生成的 iOS 编译产物并非 FAT 二进制文件（无法同时兼容多种架构）。

若需在 iOS 设备测试编译产物，需配置代码签名：
- 在 `Play` 目标中设置 `CODE_SIGNING_ALLOWED` 为 `YES`。
- 在 Xcode 的「Signing & Capabilities（签名与功能）」面板中配置代码签名参数。

macOS 下编译 Vulkan 版本，需确保 `$VULKAN_SDK` 环境变量指向正确路径。

iOS 下编译 Vulkan 版本，需在 CMake 命令中添加：
 ```bash
 -DCMAKE_PREFIX_PATH=$VULKAN_SDK
 ```

### UNIX 系统编译 ###
若未安装 CMake 或 OpenAL 库，还需安装 Qt（建议 5.6 版本）。
可通过系统包管理器安装，例如 Ubuntu：
`apt install cmake libalut-dev qt5-default libevdev-dev libqt5x11extras5-dev libsqlite3-dev`

UNIX 系统有三种编译方式：Qt Creator、Makefile 或 Ninja：
 - QT Creator：
    - 打开项目 → 选择 `Play/CMakeLists.txt`

 - Makefile/Ninja：
   ```bash
   mkdir build
   cd build
   ```
   ```
   # 不指定 -G 会默认使用 Makefiles
   cmake .. -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_PREFIX_PATH=/opt/qt56/
   cmake --build .
   # 或使用 Ninja
   cmake .. -G Ninja -DCMAKE_PREFIX_PATH=/opt/qt56/
   cmake --build . --config Release
   ```
上述示例基于 Ubuntu Trusty 系统的 Qt5.6 回溯仓库。

注：`CMAKE_PREFIX_PATH` 指向 Qt 的 bin/libs 目录所在路径。若从 Qt 官网安装，路径可能为：`~/Qt5.6.0/5.6/gcc_64/`。

### Android 系统编译 ###
Android 版本的编译已在 macOS 和 UNIX 环境下测试通过。

可通过 Android Studio 或 Gradle 编译：
 - Android Studio：
   - 文件 → 打开项目 → 选择 `Play/build_android` 目录
   - 通过 SDK 管理器安装 NDK
     - 编辑/创建 `Play/build_android/local.properties` 文件
     - macOS：添加行 `ndk.dir=/Users/USER_NAME/Library/Android/sdk/ndk-bundle`（替换 `USER_NAME` 为你的 macOS 用户名）
     - UNIX：添加行 `ndk.dir=~/Android/Sdk/ndk-bundle`
     - Windows：添加行 `ndk.dir=C:\Users\USER_NAME\AppData\Local\Android\sdk\ndk-bundle`
     - 文件末尾需保留空行。

注：上述路径仅适用于通过 Android Studio SDK 管理器安装的 NDK，否则需手动指定 NDK 实际路径。

配置完成后即可开始编译：
 - Gradle：需提前安装 Android SDK 和 NDK（均可通过 Android Studio 安装）
   - 编辑/创建 `Play/build_android/local.properties` 文件
     - macOS：
       - 添加行 `sdk.dir=/Users/USER_NAME/Library/Android/sdk`（替换 `USER_NAME`）
       - 添加行 `ndk.dir=/Users/USER_NAME/Library/Android/sdk/ndk-bundle`（替换 `USER_NAME`）
     - UNIX：
       - 添加行 `sdk.dir=~/Android/Sdk`
       - 添加行 `ndk.dir=~/Android/Sdk/ndk-bundle`
     - Windows：
       - 添加行 `sdk.dir=C:\Users\USER_NAME\AppData\Local\Android\sdk`（替换 `USER_NAME`）
       - 添加行 `ndk.dir=C:\Users\USER_NAME\AppData\Local\Android\sdk\ndk-bundle`（替换 `USER_NAME`）
     - 文件末尾需保留空行。

注：上述路径仅适用于通过 Android Studio SDK 管理器安装的 NDK，否则需手动指定 NDK 实际路径。
配置完成后执行编译命令：
 ```bash
 cd Play/build_android
 sh gradlew assembleDebug
 ```

##### 发布版/签名编译说明 #####
通过 Android Studio 可直接「Generate Signed APK（生成签名 APK）」。

通过 Gradle 编译签名发布版，需在项目目录或 `GRADLE_USER_HOME` 目录的 `gradle.properties` 文件中定义以下变量：

 ```
 PLAY_RELEASE_STORE_FILE=/密钥文件路径/key.jks
 PLAY_RELEASE_STORE_PASSWORD=密钥库密码
 PLAY_RELEASE_KEY_ALIAS=密钥别名
 PLAY_RELEASE_KEY_PASSWORD=密钥密码
 ```

配置完成后，执行以下命令生成签名发布版：
 ```
 cd Play/build_android
 sh gradlew assembleRelease
 # Windows 系统执行
 gradlew.bat assembleRelease
 ```

### 网页浏览器版本编译 ###

网页版编译需安装 [emscripten](https://emscripten.org/)。

通过 `emcmake` 生成项目：

```
mkdir build
cd build
emcmake cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTS=OFF -DBUILD_PLAY=ON -DBUILD_PSFPLAYER=ON -DUSE_QT=OFF
```

生成完成后，执行以下命令编译 JavaScript/WebAssembly 文件：

```
cmake --build . --config Release
```

编译产物（JavaScript 胶水代码和 WebAssembly 模块）需复制到 `js/play_browser` 目录对应位置：

```
build_cmake/build/Source/ui_js/Play.js -> js/play_browser/src/Play.js
build_cmake/build/Source/ui_js/Play.wasm -> js/play_browser/public/Play.wasm
build_cmake/build/Source/ui_js/Play.js -> js/play_browser/public/Play.js
```

复制完成后，进入 `js/play_browser` 目录执行以下命令，即可在本地浏览器运行网页版：

```
npm install
npm start
```

##### 浏览器环境限制 #####
- 不支持内存页写保护，因此加载 EE 模块的游戏可能运行异常（JIT 缓存无法失效）。
- 无法控制浮点运算环境，默认使用舍入模式，部分游戏可能出现异常。
