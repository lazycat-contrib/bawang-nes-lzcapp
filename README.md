# bawang-nes-lzcapp

[小霸王游戏机](https://github.com/liangminmx/bawang-nes)（网页版 NES 模拟器）的懒猫微服打包：**只发喵喵商店，镜像模式**（`delivery.mode: mirror`）。

- **镜像**：`ghcr.io/liangminmx/80h5:latest`（nginx:alpine 静态站，自带 644 个 FC 游戏，容器内监听 **3080**）。镜像模式只做只读校验，交付时把 manifest 的镜像行改写成 `ghcr.1ms.run/liangminmx/80h5:latest` ⇄ 加速地址，不搬镜像到懒猫镜像源。
- **上游只有 `latest` 一个可变 tag**：按 skill 的标准形状处理——`bump: patch`（摘要变了就把包版本 +1 个 patch）+ `channel: custom` + `sort: created` + `tag_regex: '^latest$'` + `require_digest_match: true`（可变镜像必须校验加速镜像里是同一份）。
- **路由**：`/` → `bawang:3080`，**整站公开**（`public_path: [/]`）：纯静态游戏站，没有账号也没有隐私数据，手机/平板打开就能玩。
- **无数据卷**：游戏 ROM 都在镜像里，没有数据库、升级不丢任何东西。
- **`user: root`**：镜像默认就是 root，保持它可避免 nginx 写 `/var/cache/nginx`、`/var/run` 时的权限意外。
- **健康检查**：`wget -q -O /dev/null http://127.0.0.1:3080/`（busybox wget，镜像自带）。
- **图标**：用用户提供的 `game (1).png`（方形手柄线稿，256→512 放大）。
- 上游仓库没有 LICENSE 文件（README 说改造自 echeverra.cn/bawang），所以 `package.yml` 里不写 license 字段。

## 商店状态

- **喵喵商店**：`cloud.lazycat.app.bawang-nes` 由 Action 发布（`stores.private`）。
- **官方商店**：镜像模式不满足官方商店「镜像必须来自 `registry.lazycat.cloud`」的要求，只发喵喵商店。
