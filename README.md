# WorldsTip 旅行指南小册子 · 官方展示站（纯静态）

v0.2.1，**中英双语**。整站为**纯静态展示站**：`index.html` / `index-en.html` 单文件直出，无后端、无反代、无数据库、无框架、无 CDN。

## 环境与依赖（就这么多）

| 项 | 要求 | 说明 |
|---|---|---|
| Web 服务器 | nginx（推荐）或 Apache | 纯静态直服，**无后端进程、无反代** |
| 系统 | 任意 Linux（Debian/Ubuntu/宝塔均可） | 无 Python 运行时要求、无数据库、无 Node、无 Docker |
| 磁盘 | 约 15 MB | 站点本体 ~130 KB，`downloads/` 是绝对大头 |
| 备案 / HTTPS | 面向国内域名访问才需要 | 已有旧站在跑则已就绪 |

## 目录结构

```
agent-site/                        # 直接 rsync 到服务器站点根目录即可
├── index.html            # 展示站 · 中文（单文件，内联 CSS/JS/SVG + JSON-LD）
├── index-en.html         # 展示站 · English（同版式，与中文页共用样式段）
├── robots.txt            # 爬虫规则 + sitemap 指向
├── sitemap.xml           # 双语 sitemap（含 hreflang alternate）
├── og-cover.png          # 1200×630 社交分享封面（og:image / twitter:image）
├── nginx-agent.worldstip.com.conf  # 纯静态 nginx 配置（无反代段）
├── README.md             # 本说明
└── downloads/            # 各端技能包
```

**上传时 `downloads/` 必须与 `index.html` 同级**——下载按钮用相对路径。

## 页面结构（v0.2.1 版）

| 板块 | 内容 |
|---|---|
| Hero | 双 H1（主标题 + 副标题带 worldstip.com 外链）+ 两个 CTA + 4 项指标 |
| 能力 | 四支柱：国内全交通比选 / 住宿分房与预算 / 出境四灯 + 摩旅五门禁 / 摩旅路线与城市消费层 |
| 可信度 | stable / volatile / unknown 三档灯 + 9 处逻辑修正与 44 条反例说明 |
| 在线装 | 三个已上架市场入口卡（WorkBuddy / 千问 / SkillHub），各带**实测版本号**，另注三端仅有包无公开页 |
| 下载 | 两个 tab（v0.2.1 八包 / v0.1.49 三包），卡片含解压体积、文件数、sha256 前 16 位 |
| 适配 | 八包适配矩阵表：目录结构 / frontmatter / 专属文件 / 解压体积 |
| 上手 | 装包 → 说需求 → 拿册子 |
| 验收 | 9 项实跑数据格子 |

## 下载包清单（v0.2.1）

| 平台 | 文件 | 文件数 | 解压体积 | sha256 前 16 位 |
|---|---|---|---|---|
| WorkBuddy 技能 | `worldstip-travel-planner-v0.2.1-fused.zip` | 115 | 3 066 016 B | `02b17d6ba6265da3` |
| WorkBuddy 专家 | `worldstip-travel-planner-v0.2.1-expert-fused.zip` | 119 | 3 086 595 B | `c5ca56dc15693440` |
| 千问 AI | `worldstip-travel-planner-v0.2.1-qianwen.zip` | 109 | 2 996 973 B | `b0145390eefe739c` |
| 扣子 | `worldstip-travel-planner-v0.2.1-coze.skill` | 115 | 3 066 016 B | `02b17d6ba6265da3` |
| 百度搭子 技能 | `worldstip-travel-planner-v0.2.1-baidu-dazi.zip` | 116 | 3 068 740 B | `a7041b3c73a0232b` |
| 百度搭子 套件 | `worldstip-travel-planner-v0.2.1-baidu-dazi-plugin.zip` | 119 | 3 077 963 B | `902322dc4018eab1` |
| DataBuilder | `worldstip-travel-planner-v0.2.1-baidu_databuilder.zip` | 115 | 3 066 016 B | `4f029e3bcb043213` |
| SkillHub | `worldstip-travel-planner-v0.2.1-skillhub.zip` | 115 | 3 065 083 B | `3112607ad147ea21` |

v0.1.49 存档三包（`-skill-fixed` / `-expert-fixed` / `-qianwen`）同目录保留，页面切 tab 可见。

## 打包规则（v0.2.1 起统一）

百度以「不允许的文件类型: LICENSE」驳回后，六端包一律改为**扩展名白名单**：

- 只放行 `.md` / `.json` / `.py` / `.svg`
- `LICENSE`、`.gitignore`、`.skillhubignore` 三个无扩展名文件全部清出
- `tests/_tmp_plan.json` 等临时产物一并清出
- 开源协议改由各端 frontmatter 的 `license: MIT` 声明，不依赖文件

例外：`.codebuddy-plugin/` 与 `avatars/` 属平台元数据资产，专家包的 `avatar` 字段直接指向 PNG，不按技能白名单判。

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

## 各端上架实况（2026-11-05 抓取）

| 平台 | 地址 | 页面标注版本 |
|---|---|---|
| WorkBuddy | `workbuddy.cn/share/asset?shareCode=1vcWCDMZzdZOCyznXg4EQg` | 页面为 SPA，正文无版本字段 |
| 千问 AI | `qianwenai.com/hub/skills/@user_gc9bq/worldstip-travel-planner` | v0.1.48（落后本地一版） |
| SkillHub | `skillhub.cn/skills/user_dc997363/worldstip` | v1.0.0（独立版本序列） |
| 百度搭子 / 扣子 / DataBuilder | 无公开页 | — |

## 版本纪律

页脚固定「对外发布版本 = 本地开发包版本 = v0.2.1」。升级时：① 两页搜旧版本号（title / eyebrow / 页脚 / JSON-LD softwareVersion / 下载卡数据）；② `downloads/` 增补新包；③ `sitemap.xml` 的 lastmod 一并更新。

**注意**：不要对整页做版本号全局替换——`downloads/` 里的存档包名（如 `-v0.1.49-*`）必须保留原版本号，否则链接 404。

**当前同步状态（v0.2.1）**：
- 已同步：本站两页 + 11 个包；GitHub 源码库 `Skill-WorldsTipAgent`。
- 实测数据：回归断言 **313 项**全过（27 结构 + 204 数据引擎 + 38 融合 + 44 逻辑反例），静态质检 **A/100**，11 包结构探针全 PASS，最大包 2.94 MiB 未越 3 MiB 上限。

## 技术说明

- `index.html`：单文件约 82 KB，内联 CSS + 原生 JS，无框架、无 CDN、无外部图片。
- `index-en.html` 与中文页**共用同一段 CSS**（由 `_out/gen_en.py` 从中文页抽取样式段后拼装），改样式只需改中文页再重新生成。
- 滚动显现 / 数字滚动 / tab 切换 / `prefers-reduced-motion` 全降级。
- 下载卡用 flex 布局：8 张排成 4+4，3 张排成单行 3 张，末行永远填满不留半格。
- 已通过 Chromium 实机渲染：桌面 1440 与移动 390 两个视口均无横向溢出。

## SEO / 分享卡配置（已内置）

| 项 | 位置 | 值 |
|---|---|---|
| title / description / keywords | 两页 head | 含「千问 AI 技能 Hub」「摩旅」「房车」「出境」等主关键词 |
| canonical | head | 中文页 `https://agent.worldstip.com/`；英文页指向自身 |
| hreflang | head + sitemap | zh-Hans / en / x-default 三向互指 |
| Open Graph | 两页 head | og:title / description / image（`og-cover.png` 1200×630）/ url / locale |
| Twitter Card | 两页 head | `summary_large_image` |
| 结构化数据 | 两页内 `<script type="application/ld+json">` | SoftwareApplication（softwareVersion / downloadUrl / offers=0 / sameAs 五个入口） |
| robots / sitemap | 站点根目录 | `robots.txt` 全站放行；`sitemap.xml` 双语两 URL |

上线后建议：① Google Search Console / Bing Webmaster 提交 `sitemap.xml`；② 百度搜索资源平台提交同一 sitemap；③ 用 `https://search.google.com/test/rich-results` 验 JSON-LD；④ 微信/QQ 分享预览取决于 og 标签，`og-cover.png` 需可公网访问。
