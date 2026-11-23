# starling-（汉化 + 登录修复包）

给 Starling 0.9.0 打两个补丁再编译：**界面汉化** + **微软正版登录修复**。
工作流是手动触发的 `build-zh`，产出一个可直接安装的 arm64-v8a APK。

> 个人自用。Starling 本体版权归上游作者，这里只放补丁与构建脚本。

## 仓库里有什么

```
.github/workflows/build.yml   # build-zh：拉源码 → 打补丁 → 编译 → 上传 artifact
kit.zip                       # 构建所需的全部素材
login-friends.patch           # 登录相关补丁的独立存档（方便单独查看/复用）
README.md
```

`kit.zip` 解开是：

| 文件 | 作用 |
|---|---|
| `汉化改动.patch` | 界面文案汉化补丁 |
| `登录修复.patch` | 微软正版账号登录流程修复 |
| `CJK-Regular.ttf` / `CJK-Medium.ttf` / `CJK-Bold.ttf` | 三档字重中文字体，拷进 `crates/horizon-theme/fonts/` |
| `starling.keystore` | 签名密钥库 |

## 怎么构建

1. Actions → 左侧 `build-zh` → `Run workflow`（这个工作流**只支持手动触发**，push 不会自动跑）。
2. 等它跑完（首次要现编内核，二十多分钟）。
3. 在该次 run 页面底部下载 artifact `apk-zh`，解压即得 APK。

脚本实际做的事：拉 `starling v0.9.0` 源码 → 解 `kit.zip` → 依次 `git apply 汉化改动.patch`、`git apply 登录修复.patch` → 拷字体 → 用 kit 里的 keystore 签名 → 编 `arm64-v8a`。

## 使用与注意

- 只出 **arm64-v8a**。
- keystore 固定，覆盖升级不掉存档；但与官方版签名不同，**不能直接覆盖官方客户端**，先卸载官方版并备份游戏目录。
- 登录修复只处理客户端侧的登录握手，**账号本身仍需你有正版**；无法登录时先确认系统时间准确、网络能直连微软登录域名。
- 仓库里带 keystore，公开仓库等于公开签名私钥，建议保持 Private 或换自己的密钥。
