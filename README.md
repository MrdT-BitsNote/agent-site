# WorldsTip 旅行规划大师 · 官方展示站（纯静态）

v0.1.24，**中英双语**。整站为**纯静态展示站**：`index.html` / `index-en.html` 单文件直出，无后端、无反代、无数据库。自托管 Agent 应用不再随站部署——需要就把技能包整包下载到你自己的服务器自行部署。

## 环境与依赖（就这么多）

| 项 | 要求 | 说明 |
|---|---|---|
| Web 服务器 | nginx（推荐）或 Apache | 纯静态直服，**无后端进程、无反代** |
| 系统 | 任意 Linux（Debian/Ubuntu/宝塔均可） | 无 Python 运行时要求、无数据库、无 Node、无 Docker |
| 磁盘 | 约 5MB | 站点 4.5MB（六端技能包 4.3MB 是大头） |
| 备案 / HTTPS | 面向国内域名访问才需要 | 你已有旧站在跑则已就绪 |

## 目录结构

```
agent-site/                        # 直接 rsync 到服务器站点根目录即可
├── index.html            # 展示站 · 中文（单文件，内联 CSS/JS/SVG + JSON-LD）
├── index-en.html         # 展示站 · English（同版式，hreflang 互通）
├── robots.txt            # 爬虫规则 + sitemap 指向（已去掉 /plan/ 屏蔽）
├── sitemap.xml           # 双语 sitemap（含 hreflang alternate）
├── og-cover.png          # 1200×630 社交分享封面（og:image / twitter:image）
├── nginx-agent.worldstip.com.conf  # 纯静态 nginx 配置（无反代段）
├── README.md             # 本说明
├── downloads/            # 技能包：v0.1.24 两个 + v0.1.16 存档四个
│   ├── worldstip-travel-planner-v0.1.24.zip              # v0.1.24 千问 / 通用（首层 SKILL.md，WorkBuddy / MiniMax / Marvis 亦可用）
│   ├── worldstip-travel-planner-expert-v0.1.24.zip       # v0.1.24 Qoder 专家包（.qoder-plugin）
│   ├── worldstip-travel-planner-v0.1.21.zip              # 存档 · 千问 / 通用（v0.1.24 已取代）
│   ├── worldstip-travel-planner-expert-v0.1.21.zip       # 存档 · Qoder 专家包（v0.1.24 已取代）
│   ├── worldstip-travel-planner-v0.1.16.zip              # 存档 · 千问 / 通用
│   ├── worldstip-travel-planner-v0.1.16-coze.zip         # 存档 · 扣子（首层即 SKILL.md）
│   ├── worldstip-travel-planner-v0.1.16.skill            # 存档 · 扣子 .skill（同内容）
│   ├── worldstip-travel-planner-expert-v0.1.16-qoder.zip # 存档 · Qoder（.qoder-plugin）
│   ├── worldstip-travel-planner-expert-v0.1.16-workbuddy.zip # 存档 · WorkBuddy（.codebuddy-plugin）
│   └── worldstip-travel-planner-expert-v0.1.16.zip       # 存档 · 通用专家包
└── samples/              # v0.1.24 真实生成的小册子范本（A5 紧凑卡册）
    ├── sample-changsha-hiking.html        # 「长沙周边 · 周末早爬山」完整版
    └── sample-changsha-hiking-light.html  # 同需求紧凑版
```

**上传时 `samples/` 与 `downloads/` 必须与 `index.html` 同级**——范本内嵌与下载按钮全部用相对路径。语言切换：顶栏「中文 / EN」互链，无需服务端配置。

> 本站**不提供在线试用**：`app.py` / `workbench.html` / `toolkit/` 已从站点移除，nginx 配置也不再含任何 `proxy_pass`。想自托管 Agent 应用，下载 `downloads/worldstip-travel-planner-v0.1.16.zip`，解压后 39 个确定性脚本纯 Python 标准库、零第三方依赖，Python 3.8+ 即可跑。

## 部署（两步）

```bash
# 1. 传站点
rsync -av agent-site/ root@<你的服务器>:/var/www/worldstip-agent-site/

# 2. 装 nginx 配置（root 路径与上面目录不一致时改配置里那一行）
sudo cp nginx-agent.worldstip.com.conf /etc/nginx/sites-available/
sudo ln -sf /etc/nginx/sites-available/nginx-agent.worldstip.com.conf /etc/nginx/sites-enabled/
sudo certbot --nginx -d agent.worldstip.com     # 已有证书可跳过
sudo nginx -t && sudo systemctl reload nginx
```

不需要 systemd、不需要 cron 清理、不需要放行 8080——没有后端进程。

## 页面结构（v0.1.24 版）

| 板块 | 内容 |
|---|---|
| Hero | 双 H1（SKILL → worldstip.com ／ 副标题 → 千问 Hub）+ 两个 CTA |
| ★ 千问 AI 技能 Hub | 全宽巨幅主推卡片：地址、安装指令（可复制）、4 项数据 |
| 01 它做什么 | 输入输出对照 + 4 张能力卡（交通 / 摩旅 / 房车出境 / 可信度） |
| 02 真实范本 | **一个案例**（长沙周边周末早爬山），内嵌 iframe + 完整版 / 紧凑版链接 |
| 03 行前体检 | 约束对照表 + 三态灯（红 / 黄 / 灰）+ 三条判定纪律 |
| 04 数据底座 | 6 项数字 + 4 行数据源口径表 |
| 05 两种用法 | 形态 A · SKILL ／ 形态 B · 自托管 AGENT |
| 06 安装下载 | 5 个平台卡 + 下载矩阵（最新 v0.1.24 两项 + v0.1.21 存档两项 + v0.1.16 存档四项）+ GitHub 备份入口 |
| 07 常见问题 | 5 条（含「摩托车能不能上高速」，与 JSON-LD FAQPage 逐条一致） |

## 页面内已配置的外链（按需改）

| 位置 | 当前指向 | 检索标识 |
|---|---|---|
| H1 主标题「旅行规划大师 · SKILL」 | https://worldstip.com | `h1.h1-skill a` |
| H1 副标题 | 千问 AI 技能 Hub `@user_gc9bq/worldstip-travel-planner` | `h1.h1-site a` |
| 顶栏 / 目录 / 尾屏主 CTA | 千问 AI 技能 Hub | 搜 `qianwenai.com/hub/skills` |
| ZIP 下载 | `downloads/worldstip-travel-planner-v0.1.24.zip` | 搜 `downloads/worldstip-travel-planner-v0.1.24` |

## 版本纪律

页脚已固定「对外发布版本 = 本地开发包版本 = v0.1.24」。任何一端改动后，**本地 / 千问 / 扣子 / Qoder / 乐享 / 本站六端同步同一版本号**。升级时：① `index.html` + `index-en.html` 搜旧版本号（title / badge / 页脚 / JSON-LD softwareVersion / ZIP 名）；② 范本从技能包 `examples/` 覆盖 `samples/`；③ `downloads/` 各端包、`og-cover.png`、`sitemap.xml` 的 lastmod 一并更新。

**注意**：不要对整页做版本号全局替换——`downloads/` 里的存档包名（如 `-v0.1.16-coze.zip`）必须保留原版本号，否则链接 404。先确认哪些包名实际存在，再定向替换。

**当前同步状态（v0.1.24）**：
- 已同步：**GitHub 源码库**（https://github.com/MrdT-BitsNote/Skill-WorldsTipAgent ，SKILL.md / scripts / references / examples 全量 + `releases/` 各版本 zip）、千问 / 通用技能包、Qoder 专家包。
- 待重打：WorkBuddy 专家包（`.codebuddy-plugin`）、通用专家包、扣子 `-coze.zip` / `.skill`——三者仍为 v0.1.16；其中扣子与 WorkBuddy 可直接用 v0.1.24 通用包（首层即 SKILL.md）。
- 实测数据（v0.1.24）：出境基线 32 国/区、回归断言 **956 项**全过（geo 233 + moto_check 489 + moto_enrich 98 + party 54 + rv 64 + sync 18）、报告 **10 章**（§01–§10；§02 内新增「出行查询指引」子块）。

改 `render_compact.py` 的 `HTML_TPL` 后，必须重建 `samples/` 里的范本——文档与产物对不上是最容易漏的坑。

## 技术说明

- `index.html`：单文件约 62KB，内联 CSS + 原生 JS，无框架、无 CDN、无外部图片；左栏粘性目录 + 编号分区 + 密集表格；滚动显现 / 数字滚动 / 手风琴 / scrollspy；`prefers-reduced-motion` 全降级。
- 全部视觉素材为真实小册子 iframe 与内联 SVG，无占位图。
- 已通过 Edge 实机渲染 + 链接可达性校验。

## SEO / 分享卡配置（已内置）

| 项 | 位置 | 值 |
|---|---|---|
| title / description / keywords | `index.html` / `index-en.html` head | 含「千问 AI 技能 Hub」「摩旅」「房车」「出境」等主关键词 |
| canonical | head | 中文页 `https://agent.worldstip.com/`；英文页指向 `index-en.html` 自身 |
| hreflang | head + sitemap | zh-CN / en / x-default 三向互指 |
| Open Graph | 两页 head | og:title / description / image（`og-cover.png` 1200×630）/ url / locale |
| Twitter Card | 两页 head | `summary_large_image` |
| 结构化数据 | 两页内 `<script type="application/ld+json">` | WebSite + Organization + SoftwareApplication（softwareVersion / featureList / downloadUrl / offers=0）+ BreadcrumbList（中文页）+ FAQPage（5 问答，与页面可见 FAQ 逐条一致） |
| robots / sitemap | 站点根目录 | `robots.txt` 全站放行（无 `/plan/` 屏蔽）；`sitemap.xml` 双语两 URL |

上线后建议：① 在 Google Search Console / Bing Webmaster 提交 `sitemap.xml`；② 百度搜索资源平台提交同一 sitemap（国内主要流量）；③ 用 `https://search.google.com/test/rich-results` 验 JSON-LD 是否被识别；④ 微信/QQ 分享预览取决于 og 标签，`og-cover.png` 需可公网访问。
