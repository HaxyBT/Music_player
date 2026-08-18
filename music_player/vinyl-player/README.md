# Vinyl · 黑胶唱片播放器

一个极简风格（黑白灰 + 点缀金）的**本地音乐播放器**，纯 HTML/CSS/JS，零构建、无后端。

## ✨ 核心特性

- **本地音乐上传**：点击「＋ 添加音乐」或直接拖拽音频文件到页面（支持 MP3 / FLAC / M4A / AAC / OGG / WAV）。
- **专辑封面 + 黑胶旋转**：用 [jsmediatags](https://github.com/aadsm/jsmediatags) 自动读取音频内嵌封面（ID3 APIC / FLAC Picture），中央封面以**黑胶唱片形式旋转**展示；播放转动、暂停定格。
- **歌词跟随滚动**：
  - 拖入**同名 `.lrc` 歌词文件**（如 `song.mp3` + `song.lrc`）→ 逐行同步、当前句高亮放大并平滑居中滚动；
  - 无 `.lrc` 时自动回退读取音频内嵌歌词（ID3 USLT）；
  - 无歌词则显示「暂无歌词」。
- **播放控制**：播放/暂停、上一首/下一首、进度条拖动、音量调节、播放队列（多首时自动出现，点击切歌）。
- **多文件**：可一次拖入多首歌曲，自动组队循环播放。
- **隐私安全**：文件全部在浏览器本地处理，**不会上传到任何服务器**。

## 🚀 使用方法

1. 直接打开 `index.html`（或用任意静态服务器，例如 `python3 -m http.server`）。
2. 拖入本地音乐文件，或点击右上角「＋ 添加音乐」。
3. 想快速看效果：打开 `index.html#demo`，或在空状态页点「或先试听示例」，会加载内置示例（带封面 + 同步歌词）。

> 提示：`jsmediatags` 从 CDN（jsdelivr，备用 unpkg）加载，首次使用需联网；音频与封面读取则完全离线。

## 📁 文件结构

```
vinyl-player/
├── index.html          # 播放器主文件（HTML + CSS + JS 单文件）
├── example-song.mp3    # 内置示例曲（含内嵌封面与标题/艺术家元数据）
├── example-song.lrc    # 示例同步歌词
├── cover.png           # 示例封面源图（金色月面渐变）
└── preview.png         # 效果截图
```

## 🎛️ 技术要点

- **元数据解析**：`jsmediatags` 读取标题/艺术家/专辑封面（APIC）与内嵌歌词（USLT）。
- **LRC 解析**：支持 `[mm:ss.xx]` / `[mm:ss.xxx]` 时间戳，多时间标签合并，按时间排序。
- **歌词跟随**：`timeupdate` 中二分定位当前句，高亮 + `scrollTo({behavior:'smooth'})` 居中。
- **黑胶旋转**：CSS `@keyframes spin` + `animation-play-state`（暂停时定格在当前位置，而非跳回）。
- **隐私**：`URL.createObjectURL` + `URL.revokeObjectURL` 本地对象 URL，文件不上传。
