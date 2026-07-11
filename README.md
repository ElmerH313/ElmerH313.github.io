# Elmer.H 个人主页（Web 项目 Wiki）

> 独立开发者 Elmer.H 的个人品牌站：应用下载 + 隐私政策托管 + 未来更多应用的展示位。
> 本文件是唯一的开发文档，新 session 从这里接手即可独立维护。

## 项目定位

- **个人账号为主**的主页，不是单一 App 的落地页；Apps 区当前只有 GuitarMetro，预留了"更多工具打磨中"占位卡
- 顺带承担 **GuitarMetro 各商店上架所需的隐私政策 URL**（`privacy.html`，中英双语、零收集声明）
- 纯静态、零构建、零依赖（唯一外部资源：Google Fonts 的 Baloo 2 + Patrick Hand）

## 品牌规范（涂鸦贴纸风）

**一切配图跟随 Logo 风格**（用户原话）。Logo 源文件：`A:\_Myself_\LOGO\ElmerH\`
（PSD 源 + `LOGO_V01.png` 圆形徽章版 / `LOGO_V01_NoFrame.png` 平底版；站内用的是 NoFrame，已拷至 `assets/logo.png`）。

| 语义 | 值 | 来源 |
| --- | --- | --- |
| 天蓝底 `--sky` | `#6DC4DC` | Logo 背景色（取样自图像，页面与 Logo 无缝同色） |
| 深蓝地面 `--sky-deep` | `#579CAF` | Logo 底部色带；用作页脚 |
| 深墨文字 `--ink` | `#253444` | |
| 粉 `--pink` | `#E14FB2` | Logo 涂鸦主色 |
| 紫 `--purple` | `#8A63E8` | |
| 紫红 `--violet` | `#B44FD8` | |
| 橙 `--orange` | `#F0993F` | |
| 薄荷 `--mint` | `#43CFA4` | Android 按钮 |

**贴纸语言**：白底圆角卡 + 硬投影（`0 10px 0 rgba(37,52,68,.18)`）+ 微旋转（±0.5°~3°）+ 半透明"胶带"贴条（`::before`）；标题用手写体 Patrick Hand 做成深色药丸标签；正文/标题字体 Baloo 2。

**Logo 使用注意**：PNG 不透明（底色即页面色）——**不要加 drop-shadow**（会暴露矩形边界）；hero 处用 `aspect-ratio: 1/0.84 + overflow:hidden` 裁掉了底部地面带。

## 文件结构

```
index.html      主页（导航/Hero/Apps/About/页脚，内联全部 CSS/JS）
privacy.html    GuitarMetro 隐私政策（商店上架用 URL）
assets/
  logo.png              品牌 Logo（NoFrame 版拷贝）
  guitarmetro-icon.png  App 图标（源：GtMetronome 仓库 assets/icon/app_icon.png）
  shot-dial.png/shot-mini.png/shot-tuner.png  应用截图（英文界面）
  favicon.svg           旧 LED 风 favicon（已弃用，现用 logo.png，可删）
```

## 双语机制

默认英文；`navigator.language` 以 zh 开头则自动切中文；右上角按钮手动切换并 `localStorage` 记忆。
写法：每段文案成对出现 `<span class="en-t">…</span><span class="zh-t">…</span>`，靠 `body.zh` 类切换显隐。**新增任何文案必须双语成对。**

## 本地预览

```powershell
cd A:\_Myself_\__ElmerH_Home__
python -m http.server 8642
# 浏览器打开 http://localhost:8642
```
截图验证（内置浏览器不稳时）：
`msedge --headless --disable-gpu --screenshot=out.png --window-size=1240,2600 http://localhost:8642`

## 发布（🔴 当前最重要待办）

目标：GitHub Pages 用户主站。**网址由 GitHub 用户名决定**：

1. ✅ 2026-07-11 账号已改名：**Houmj0313 → `ElmerH313`**（用户自行操作；本地两个仓库 remote 已同步新地址，凭据不受影响）→ 站点网址将是 **`https://elmerh313.github.io`**（Pages 域名自动小写）
2. 🔴 新建**公开**仓库，名字精确为 **`ElmerH313.github.io`**（网页端创建；AI 直接 API 建公开仓库会被安全策略拦截，需用户操作或明确授权）
3. 本目录 `git push` 到该仓库 main 分支 → Pages 自动生效
4. ✅ `index.html` 下载链接已指向
   `https://github.com/ElmerH313/ElmerH313.github.io/releases/download/guitarmetro-v0.6.0/...`（2026-07-11 已替换，版本号 v0.6.0）
5. 创建 Release（tag 精确为 `guitarmetro-v0.6.0`，与链接一致）并上传安装包，让下载按钮生效：
   - `A:\_Myself_\__GtMetronome__\dist\GuitarMetro-win64.zip`
   - `...\GuitarMetro-macOS.zip`（已公证）
   - `...\GuitarMetro-android.apk`（侧载版）
   - iOS 按钮已直连 TestFlight：https://testflight.apple.com/join/YmKxWtsj
   - GitHub token 可从凭据管理器取：`printf "protocol=https\nhost=github.com\n\n" | git credential fill`（password= 行）
6. GuitarMetro 出新版后：上传新 Release 资产 + 更新 index.html 的版本号与链接 tag

## 待办清单

- [x] 2026-07-11 GitHub 账号改名 → ElmerH313；两仓库 remote、主页链接均已换新
- [ ] 用户创建 `ElmerH313.github.io` 公开仓库
- [ ] push 上线 → 挂 v0.6.0 安装包（见上）
- [ ] GuitarMetro v0.6.0 发布后同步更新下载区（版本号、新功能卖点截图）
- [ ] 微软商店 / App Store 上架后：把"即将上架"占位按钮换成真实商店徽章链接
- [ ] （可选）自定义域名：买 elmerh.dev / elmerhou.com 之类 → 仓库 CNAME + DNS
- [ ] （可选）About 区文案由用户润色；补社交链接
- [ ] （可选）删除已弃用的 assets/favicon.svg

## 关联项目

- GuitarMetro 仓库：`A:\_Myself_\__GtMetronome__`（私有，github.com/ElmerH313/myGtMetro）
  其 wiki 在 `docs/`；本站截图/图标来自该项目，App 有大更新时记得回来同步下载区。
