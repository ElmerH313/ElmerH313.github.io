# Elmer.H 产品入口（Web Hub）

> `https://elmerh.com` 的静态产品入口页。GitHub Pages 从 `main` 分支直接发布。

当前主页验收版本：`1.0.1`。

## 仓库职责

本仓库只负责统一入口体验，不包含任何产品源码、产品构建产物、完整营销页面、手册或下载发布逻辑。

| 公开路径 | 所属仓库 | 本仓库职责 |
| --- | --- | --- |
| `/` | `ElmerH313/ElmerH313.github.io` | 品牌首屏、纵向产品入口、语言切换和页脚 |
| `/guitarmetro/` | `ElmerH313/guitarmetro` | 仅提供入口图标、摘要和链接 |
| `/fingerboard/` | `ElmerH313/fingerboard` | 仅提供入口图标、摘要和链接 |

每个产品仓库独立维护源码、测试、产品站、截图、手册、下载、产品隐私政策和 GitHub Pages 工作流。未来产品继续使用“一项产品一个仓库”，且仓库名必须与 `elmerh.com/<slug>/` 的 `slug` 一致。

## 主页内容边界

主页只展示：

- Elmer.H 大型品牌 Logo、名称与一句个人描述。
- GuitarMetro 和 Fingerboard Web 两个纵向产品入口。
- 无手动选择时按浏览器首选语言自动显示中文或英文；手动选择保存到 `localStorage('lang')` 并始终优先。
- GitHub、Email、根隐私政策和版权信息。

主页仅保留一句个人描述，不提供社交二维码、留言板、产品截图、功能清单、下载按钮或产品手册内容。入口图标是主页自有的标准化展示副本，产品运行时不从本仓库读取资源。

## 兼容路径

- `/privacy.html` 保持现有内容和 URL，继续兼容已经发布到商店的隐私政策链接。
- `/manual.html` 是兼容跳转页，跳转到 `/guitarmetro/manual/`。
- `downloads/` 暂时作为历史发布副本保留，但主页不再提供入口；确认外部链接完成迁移后再单独清理。

## 设计与可访问性

- 暗色工作室风，字体全部从 `assets/fonts/` 同域加载，不引用 Google Fonts。
- 桌面端双列、`≤760px` 单列，并为 `≤420px` 手机宽度收紧间距。
- 正文基准为 16px，交互控件具备可见键盘焦点。
- 非必要动画不超过 180ms，并响应 `prefers-reduced-motion`。
- 页面支持鼠标、键盘和触控；不依赖悬停展示关键信息。

## 文件结构

```text
index.html                       产品入口页（内联 CSS/JS）
manual.html                      GuitarMetro 手册兼容跳转
privacy.html                     根隐私政策兼容页
downloads/                       历史安装包，暂不删除
CNAME                            自定义域名
assets/
  fonts.css / fonts/             自托管字体
  logo.png / logo-mark.png       品牌与社交分享图
  guitarmetro-icon.png           GuitarMetro 入口图标
  fingerboard-web-icon.png       Fingerboard Web 入口图标
```

## 本地预览与验证

```powershell
python -m http.server 8642
```

打开 `http://127.0.0.1:8642/`。独立产品仓库未挂载到同一本地服务器时，两个产品路径返回 404 属于预期；生产环境必须分别验证：

1. `https://elmerh.com/`
2. `https://elmerh.com/guitarmetro/`
3. `https://elmerh.com/fingerboard/`
4. `https://elmerh.com/privacy.html`

主页修改还需检查中英文切换、360/768/1440px 布局、Tab 焦点顺序、减少动画模式，并确认源码中不存在 Supabase、留言板、二维码或产品截图引用。
