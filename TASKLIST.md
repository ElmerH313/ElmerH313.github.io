# Elmer.H 项目待办

## Homepage 1.1.1：Chord Diagram 规范入口

- [x] 将 Chord Diagram 入口更新为 `/ChordDiagram/Lookup`。
- [x] 验证 `/ChordDiagram/Lookup`、`/ChordDiagram/Identifier` 与产品页返回主页按钮。

## 当前状态：海外站恢复，中国大陆接入继续暂停

- [x] 停止腾讯云服务器上的 Nginx。
- [x] 保持腾讯云防火墙 HTTP（80）规则关闭。
- [x] 将 GitHub Pages 从临时空分支 `codex/icp-review-offline` 切回 `main`。
- [x] 重新绑定 GitHub Pages 的 `elmerh.com` 自定义域名并恢复 HTTPS。
- [x] 验证 `/`、`www`、`/guitarmetro/`、`/fingerboard/` 和 Chord Diagram 均可访问。
- [x] 删除本机预览隧道及临时 SSH 密钥。

> 在 `elmerh.cn` 完成备案前，不得启动腾讯云 Nginx，也不得开放中国大陆服务器的 HTTP（80）与 HTTPS（443）端口。

## elmerh.cn 备案通过后恢复中国大陆接入

- [ ] 启动腾讯云 Nginx，并验证 `/`、`/guitarmetro/`、`/fingerboard/`。
- [ ] 根据正式域名接入方案恢复腾讯云 HTTP（80）与 HTTPS（443）规则。
- [ ] 确认国内与海外 DNS 解析目标正确，避免切换期间中断海外访问。
- [ ] 完成公网、手机网络、安装包下载和 Range 断点续传验收。

## COS / CDN

- [x] 开通腾讯云 COS；当前资源包总容量显示为 150 GB。
- [ ] 确认 COS 存储桶地域：南京单 AZ（推荐，可使用资源包）或上海多 AZ（资源包不抵扣且成本更高）。
- [ ] 创建私有读写存储桶。
- [ ] 上传 GuitarMetro 安装包和适合 CDN 分发的静态资源。
- [ ] 上传 Fingerboard Web 静态资源，并保持独立仓库与 `/fingerboard/` 路由边界。
- [ ] 配置 CDN 回源 COS、HTTPS、自定义域名、缓存规则与 Range 回源。
- [ ] 配置版本化文件名、SHA-256 校验和长缓存策略。
- [ ] 设置流量、请求量和费用告警，避免超出资源包后意外按量计费。

## 当前已部署到腾讯云但未对外开放

- Homepage：海外 GitHub Pages 目标版本为 `1.1.1`；中国大陆服务器仍为 `1.0.1`，目录 `/var/www/elmerh.com`。
- GuitarMetro：`1.0.8+9`，服务器路径 `/guitarmetro/`。
- Fingerboard Web：`0.1.2`，服务器路径 `/fingerboard/`。
