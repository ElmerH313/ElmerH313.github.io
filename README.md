# Elmer.H 个人主页（Web 项目 Wiki）

> 独立开发者 Elmer.H 的个人品牌站：应用下载 + 隐私政策托管 + 留言板。
> 本文件是唯一的开发文档，新 session 从这里接手即可独立维护。
> **线上地址：https://elmerh.com**（GitHub Pages + 自定义域名，2026-07-11 上线）

## 项目定位

- **个人账号为主**的主页，不是单一 App 的落地页；Apps 区当前只有 GuitarMetro，预留"更多工具打磨中"占位卡
- 承担 **GuitarMetro 各商店上架所需的隐私政策 URL**：`https://elmerh.com/privacy.html`（英文为主、中文可切换、零收集声明）
- 纯静态、零构建；**唯一运行时外部依赖是 Supabase（留言板）**，其余资源（含字体）全部同域自托管

## 部署与域名（已全部生效）

| 项 | 值 |
| --- | --- |
| 仓库 | `github.com/ElmerH313/ElmerH313.github.io`（公开），push main 即发布 |
| 正式域名 | `elmerh.com`（仓库 `CNAME` 文件），强制 HTTPS，证书 GitHub 自动续 |
| DNS（域名商面板） | `@` → A 记录 ×4：`185.199.108/109/110/111.153`；`www` → 同 4 个 A 记录 |
| 跳转关系 | `www.elmerh.com` 和旧 `elmerh313.github.io` 均 301 → `https://elmerh.com` |

## 🇨🇳 中国大陆可达性（重要，改动前必读）

本站能被国内访问是精心调整的结果，**破坏以下任意一条都会让国内用户"打不开"**：

1. **禁止引用 Google Fonts / 任何谷歌系资源**——`fonts.googleapis.com` 在国内被 DNS 污染到假 IP，渲染阻塞的字体 CSS 会导致白屏卡死。字体已抓成 latin woff2 自托管（`assets/fonts/` + `assets/fonts.css`），7 个字面共约 220KB
2. **安装包必须同域直发**（`/downloads/`），**不要用 GitHub Release 链接当主下载**——Release 资产走 `objects.githubusercontent.com`，国内基本拉不动；而 `elmerh.com`（Fastly）网页能开就能下文件
3. `*.github.io` 域名本身在国内部分封锁（阿里 DNS 直接 SERVFAIL），所以对外**只宣传 elmerh.com**
4. Supabase（Cloudflare 节点）国内可达；留言板加载失败会静默回退 `assets/messages.json`，不会白屏

## 设计规范（暗色工作室风，2026-07-11 全面改版）

旧的"涂鸦贴纸/天蓝底"风格已废弃。现行：

| 语义 | 值 |
| --- | --- |
| 页面底色 `--bg` | `#0a0d13`（顶部叠极淡紫/粉径向光晕） |
| 面板 `--panel` / `--panel-2` | `#11141d` / `#151926`（1px 描边 `rgba(255,255,255,.08)`） |
| Hero Logo 底衬 | **纯色 `#0f1218`、无描边**、圆角 34px，包裹 Logo + ELMER.H 字标（用户多次调过，勿动） |
| 强调色 | 粉 `#f45fc0` · 紫 `#9d7bff` · 青 `#58d6ff` · 薄荷 `#3fd6a5` · 橙 `#f0993f` |
| 字体 | 标题 **Unbounded**（600/800）· 正文 **Space Grotesk**（400/500/700）· 标签/代码感 **JetBrains Mono**（400/600），全部自托管 |

**Logo**：`assets/logo-mark.png` 为透明底抠图版（脚本洪泛去蓝底/外发光/底部手写字，源图 `assets/logo.png` 保留用于 og:image）。透明版可加 drop-shadow 霓虹投影。
**区块标题**：JetBrains Mono 大写 `// APPS` + 渐隐分割线。**下载按钮**：深色描边 + 各平台霓虹色光点。

## 语言机制（用户明确要求）

- **默认永远英文**，不做浏览器语言自动检测；仅右上角按钮手动切换，`localStorage('lang')` 记忆
- **About 自我介绍永远只有英文**（含名字），没有中文版本——这是特例，别"补全"它
- 其余文案成对 `<span class="en-t">…</span><span class="zh-t">…</span>`，靠 `body.zh` 切换；privacy.html 用 `.zh` 段落 + `body.zh-on`

## 留言板（Supabase，即发即显 + 事后审核）

- 数据库：Supabase 项目 `rkuranahncwlllerdomu`，表 `public.guestbook`（name ≤24 / text ≤200 / created_at / hidden）
- RLS：匿名仅可 SELECT（`hidden=false`）和 INSERT；无 update/delete 策略 → 访客改不了删不了
- 页面内嵌 `sb_publishable_...` key（设计上公开，安全）；**`sb_secret_` 密钥绝不能出现在任何文件/聊天中**（2026-07-11 曾在聊天泄露过一次，已提醒用户作废）
- 防滥用：蜜罐字段 `#gbWeb` + 同端 30 秒节流 + 双端长度限制
- **审核 = 事后删除**：Supabase Dashboard → Table Editor → `guestbook` → 勾 `hidden` 或删行
- UI：两行高滚动区，>2 条时每 3.2s 上滚一条循环（setTimeout 兜底，勿改回 transitionend——后台标签页不触发会卡住）
- 展示条目是 `white-space:nowrap` 省略号单行——依赖 `.app-card > * { min-width:0 }` 防止撑破网格（重要教训，见下）

## 安装包直发（⭐ 每次发版都要做）

**主下载渠道**是本仓库 `downloads/` 目录（同域，国内外通用）：

```
downloads/GuitarMetro-win64.zip      ← 文件名必须与 index.html 按钮 href 完全一致
downloads/GuitarMetro-macOS.zip     （已公证版）
downloads/GuitarMetro-android.apk   （侧载版）
```

**发新版流程**：
1. App 仓库打包产物在 `A:\_Myself_\__GtMetronome__\dist\`（文件名规范见该仓库 `docs/06-发布渠道.md`）
2. 覆盖拷贝到本仓库 `downloads/` → 更新 `index.html` 下载区版本号与中英双语更新说明（搜 `Download · v`）→ commit + push
3. （可选镜像）同步建 GitHub Release：tag `guitarmetro-vX.Y.Z`，API 上传同名资产；token 取法：`printf "protocol=https\nhost=github.com\n\n" | git credential fill`（password= 行）
4. 实测三个 `https://elmerh.com/downloads/...` 链接 200 再收工

**限制**：单文件 <100MB（当前 APK 62MB 最大）；**禁用 Git LFS**（Pages 不解析 LFS，会发布成指针文本）；超限时迁移对象存储再议。

## 响应式（两个断点 + 一个血泪教训）

- `≤820px`：App 卡片双栏 → 单栏
- `≤600px`：导航收紧（适配到 320px）、Logo/标题缩小、截图改"大图通栏+两小图并排"、留言表单全宽、privacy 同步有一套
- Hero 下方的 8-bit 贪食蛇只允许 `(min-width:900px) + hover:hover + pointer:fine` 的桌面设备显示；CSS 默认隐藏、JS 同条件拒绝初始化，移动端不占空间。控制：小键盘 8/2/4/6（兼容普通方向键），小键盘 0 开始/暂停/重开；最高分存 `localStorage.snakeBest`
- **教训**：CSS Grid 子项默认 `min-width:auto`，任何 nowrap 长内容（如长留言）会把卡片撑到 600px+，手机端整页缩小裁切。已用 `.app-card > * { min-width:0 }` 修复——新增网格/弹性布局时记得这条

## 文件结构

```
index.html        主页（内联全部 CSS/JS：语言切换 + 留言板 + 桌面贪食蛇）
privacy.html      GuitarMetro 隐私政策（商店上架 URL）
CNAME             自定义域名 elmerh.com（GitHub 生成，勿删）
downloads/        安装包直发目录（发版时更新）
assets/
  fonts/ fonts.css       自托管字体（latin woff2 ×7）
  logo.png               原始 Logo（蓝底，og:image 用）
  logo-mark.png          透明底抠图版（页面实际使用）
  guitarmetro-icon.png   App 图标
  shot-dial/mini/tuner.png  应用截图（英文界面）
  messages.json          留言板离线兜底（Supabase 挂了才显示）
  favicon.svg            已弃用，可删
.claude/launch.json      本地预览配置（python http.server 8642）——已 gitignore
```

## 本地预览

```powershell
cd A:\_Myself_\__ElmerH_Home__
python -m http.server 8642   # http://localhost:8642
```
注意：Claude 内置预览面板最窄只能模拟 ~705px，**手机断点（≤600px）问题测不出来**，移动端问题让用户真机截图。

## 待办清单

- [x] 2026-07-11 全部完成：建仓上线 → 暗色改版 → 留言板（邮件→Supabase）→ 字体自托管 → 自定义域名 elmerh.com + HTTPS → 移动端适配 ×2 → Release + 同域直发
- [ ] 微软商店 / App Store 上架后：把"soon"占位按钮换成真实商店徽章链接
- [ ] GuitarMetro 出新版：按"安装包直发"流程更新（版本号 + 三个包）
- [ ] （可选）删除 assets/favicon.svg；About 文案润色；补社交链接（吉他频道）

## 关联项目

- GuitarMetro 仓库：`A:\_Myself_\__GtMetronome__`（私有，github.com/ElmerH313/myGtMetro）
  其 wiki 在 `docs/`（发布渠道见 `06-发布渠道.md`，已含对本站直发流程的引用）；本站截图/图标来自该项目
