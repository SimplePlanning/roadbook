# 路书 · 高德导航分享

单文件网页工具：电脑上规划自驾路书（标点、排序、路线预览），生成分享链接发到微信，手机点开逐段调起高德 App 导航。零后端，路书数据全部编码在链接里。

## 在线地址

```
https://simpleplanning.github.io/roadbook/
```

托管在 GitHub Pages（仓库 `SimplePlanning/roadbook`，`gh-pages` 分支），国内访问正常，永久有效，不依赖任何个人电脑开机。

## 文件结构

```
/home/wanji/roadbook/
├── index.html   # 全部功能（页面 + 样式 + 逻辑），单文件零依赖
├── README.md
└── .git/        # gh-pages 分支，推送到 GitHub 即更新线上页面
```

## 使用方法

1. **首次使用配置高德 Key**（一次性，存在浏览器 localStorage，换浏览器/换域名要重填）：
   到 [lbs.amap.com](https://lbs.amap.com) 注册开发者 → 创建应用 → 添加「Web端(JS API)」类型 Key → 把 Key 和安全密钥填进页面弹窗
2. **标点**：顶部搜索框搜地点，结果行点击定位、点 `+` 加入；或直接点地图，弹出的气泡里点 `+` 加入。最多 50 个点
3. **调整**：左侧列表可改名、上移/下移、删除；地图上标记可拖动微调；相邻点之间自动画驾车路线（分段缓存，失败的段显示虚线并给出提示条，点提示条可重试）
4. **保存分享**：点「保存」生成链接和二维码，发到微信
5. **手机导航**：点开链接是路书列表，逐段点「导航」调起高德 App

## 已知限制

- **微信内置浏览器无法直接拉起高德 App**（平台限制）：需按页面顶部提示「在浏览器打开」后再点导航
- 高德导航 URI 只支持单目的地，所以路书是"逐段导航"形式；点名称建议写成 `D1 xxx` 便于区分
- 驾车规划走的是高德 JS API 配额（个人 Key 约 5000 次/日），分段已做缓存+防抖+限并发，正常使用没问题；配额耗尽时路段会显示虚线，控制台（F12）搜"路书"可看具体错误
- 直接双击 `index.html`（file://）打开可以规划，但点「保存」会被拦截提醒——因为 file:// 链接手机访问不到

## 更新部署

改完 `index.html` 后推送即上线（约 1 分钟生效）：

```bash
cd /home/wanji/roadbook
git add index.html && git commit -m "更新说明"
GIT_SSH_COMMAND="ssh -i ~/.ssh/github_roadbook -o IdentitiesOnly=yes" git push
```

- SSH 私钥：`~/.ssh/github_roadbook`（公钥已添加到 GitHub 账号 Settings → SSH keys，Title 为 `roadbook`）
- 注意：办公室网络屏蔽了 github.com 网页版（443），但 SSH（22）和 API 正常，推送不受影响；github.io 访问正常

## 历史说明

- 2026-09-17 初版上线：曾用 `python3 -m http.server`（局域网）+ cloudflared 临时隧道（公网）过渡，迁移到 GitHub Pages 后均已关停
- cloudflared 二进制留在 `~/.local/bin/cloudflared`，如需临时公网分享可再用：
  `cloudflared tunnel --url http://localhost:8080`（免费、免注册，但每次重启域名会变）
