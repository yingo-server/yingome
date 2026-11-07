# YingoTab 默认音乐仓库

这是 [YingoTab](https://github.com/yingo-server) 的默认音乐仓库，内置 6 首曲目，供 YingoTab 实例开箱即用。仓库结构与 [yingo-template](https://github.com/yingo-server/yingo-template) 完全相同，由 GitHub Actions 自动生成清单文件 `yingotab.yml`。

## 目录结构

```
├── apps/            # 应用目录（本仓库默认为空）
├── music/           # 音乐目录（.mp3/.flac/.ogg/.m4a）
│   └── music.json   # 曲目元数据表 {id: {title, artist}}
├── scripts/gen-yml.mjs       # 清单生成脚本
└── .github/workflows/manifest.yml
```

## 如何添加应用

在 `apps/` 下新建以应用 slug 命名的子目录，放入两个文件：

1. `apps/<slug>/app.json`：

```json
{
  "id": "example-app",
  "name": "示例应用",
  "icon": "🧩",
  "type": "dock",
  "entry": "apps/example-app/index.html"
}
```

2. `apps/<slug>/index.html`：应用入口页面。

如果缺少 `app.json`，CI 会用目录名兜底生成默认元数据。

## 如何添加音乐

把音频文件（`.mp3` / `.flac` / `.ogg` / `.m4a`）放进 `music/` 目录即可。可选：编辑 `music/music.json` 提供元数据表，键为文件名去扩展名后的 slug：

```json
{
  "city-in-rain": { "title": "雨中城", "artist": "某人" }
}
```

未在元数据表中的曲目，标题将使用文件名。

## CI 用法

进入仓库的 **Actions** 页面，选择 **manifest** workflow，点击 **Run workflow** 手动触发。CI 会运行 `scripts/gen-yml.mjs` 重新生成 `yingotab.yml`，若有变更则自动提交并推送。

## yingotab.yml 字段说明

| 字段 | 说明 |
| --- | --- |
| `apps[].id` | 应用唯一标识 |
| `apps[].name` | 应用显示名称 |
| `apps[].icon` | 应用图标（emoji 或 URL） |
| `apps[].type` | 展示位置：`dock` 或 `grid` |
| `apps[].entry` | 应用入口 HTML 路径 |
| `music[].id` | 曲目 slug |
| `music[].title` | 曲目标题 |
| `music[].artist` | 曲目艺术家 |
| `music[].file` | 音频文件路径 |
| `widgets` | 预留字段，当前固定为 `[]` |

## 在 YingoTab 实例中配置本仓库

在你的 YingoTab 实例的设置页面中填入本仓库 URL，支持三种模式：

1. **jsdelivr**：`https://cdn.jsdelivr.net/gh/<owner>/<repo>@main/yingotab.yml`
2. **ghproxy**：`https://proxy.344977.xyz/https://raw.githubusercontent.com/<owner>/<repo>/main/yingotab.yml`
3. **pages**：开启 GitHub Pages 后填入 `https://<owner>.github.io/<repo>/yingotab.yml`

## 许可证

[Apache-2.0](./LICENSE)
