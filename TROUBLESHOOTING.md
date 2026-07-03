# 静态页面更新后手机浏览器不生效

## 问题现象

修改了 `index.html` 中的比赛信息（时间、球队），本地文件已确认更新，但手机浏览器打开 GitHub Pages 链接后显示仍是旧内容。

## 排查过程

1. **怀疑代码没改上** → 确认本地文件内容已更新
2. **怀疑浏览器缓存** → 让用户清除缓存 / 强制刷新 / 隐私模式，均无效（手机浏览器对静态文件缓存非常激进）
3. **发现根本原因** → 用户通过微信收藏夹保存的链接打开，微信/手机浏览器会强缓存 HTML，即使内容已更新也不拉取新版本

## 解决方案

### 1. 加禁用缓存的 meta 标签（预防）

```html
<meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">
<meta http-equiv="Pragma" content="no-cache">
<meta http-equiv="Expires" content="0">
```

### 2. 加版本标记（便于确认）

在页面标题加入可见的版本号，用户一眼就能判断是否加载到了新版本：

```html
<h1>🐍 贪食蛇 <span style="font-size:0.5em;color:#888;">(v2 — 阿根廷vs佛得角)</span></h1>
```

### 3. 用查询参数绕过缓存（一次性）

用 `?v=2` 参数让浏览器认为是一个新 URL，强制拉取最新版本。加载一次后缓存更新，后续用原链接即可：

```
https://liutao188.github.io/snake-game/?v=2
```

### 4. 更新到 GitHub Pages

本地改完不算，需要 commit + push 到 `main` 分支，GitHub Pages 会自动部署（通常 1-2 分钟内生效）。

## 教训

- 静态 HTML 页面要加 `Cache-Control` meta 标签，否则手机浏览器可能永远不拉新版本
- 页面留一个可见的版本号标记，调试时省很多时间
- GitHub Pages 项目，改完需要 push 才能生效，本地改了没用
