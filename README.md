# dsh-

个人 DeepSeek Harness（dsh）插件索引仓。仓库根目录的 [`index.json`](index.json) 就是 dsh **插件 → Discover** 页可以直接添加的索引源（format 1）。

## 索引地址

```
https://raw.githubusercontent.com/Cromvy/dsh-/main/index.json
```

- 必须是 `raw.githubusercontent.com` 直链。`github.com/.../blob/...` 或 `.../raw/...` 会 302 跳转，dsh 的 Host 明确拒绝重定向。
- 仓库必须保持 **public**：Discover 匿名 https 读取，不带任何凭据。
- 直连 raw 不稳定时可换等价地址（同样直接 200、不跳转）：
  - `https://cdn.jsdelivr.net/gh/Cromvy/dsh-@main/index.json`
  - `https://cromvy.github.io/dsh-/index.json`（需在仓库 Settings → Pages 打开）

## 在 dsh 里添加

1. 左侧栏 **插件（Plugins）** → **Discover** 标签
2. 没有源时点 **Add index source**；已有源时用工具栏索引菜单 → **Manage index sources…**
3. 粘贴上面的 raw 直链

## 加一个插件条目

`plugins[]` 每项写包名、标题、描述、图标，以及每个版本的来源。两种来源类型：

```json
{
  "name": "@scope/dsh-example",
  "title": { "en": "Example", "zh": "示例" },
  "description": { "en": "…", "zh": "…" },
  "author": "Cromvy",
  "homepage": "https://github.com/Cromvy/dsh-",
  "repository": { "url": "https://github.com/Cromvy/dsh-", "directory": "plugins/example" },
  "metadata": { "category": "ui", "labels": ["ui-panel"], "keywords": ["example"] },
  "icon": "plugins/example/icon.svg",
  "versions": [
    {
      "version": "0.1.0",
      "publishTimestamp": 1790000000,
      "source": { "type": "npm" },
      "engines": { "dsh": ">=0.2.1 <0.3.0" }
    },
    {
      "version": "0.1.0",
      "publishTimestamp": 1790000000,
      "source": {
        "type": "git",
        "url": "https://github.com/Cromvy/dsh-",
        "commit": "0123456789abcdef0123456789abcdef01234567",
        "path": "plugins/example"
      },
      "engines": { "dsh": ">=0.2.1 <0.3.0" }
    }
  ]
}
```

- `publishTimestamp` 是 Unix 秒。
- `versions` 按 SemVer 降序（读取端会自己排序，手写时保持降序更好读）。
- `engines.dsh` 必须覆盖正在运行的 dsh 版本，否则该版本会显示为不兼容、不提供安装。
- `metadata.category` / `labels` 必须在本文件的顶层 `metadata.categories` / `metadata.labels` 里声明过，否则 Discover 的筛选器里点不到。

改完 `index.json` 提交推送即可，raw 地址立刻是新内容；jsDelivr 有缓存，急用可换 raw。

## 注意

- 索引里的 `name`、`version`、`repository` 只是**声明**，真正装上的内容以被安装包的 manifest 为准。
- 安装插件会在你的权限下运行第三方代码，索引本身也不带签名校验。
