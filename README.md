# WorldsTip 旅行指南小册子 · 官方展示站（纯静态）

v0.2.276，**中英双语**。整站为**纯静态展示站**：`index.html` / `index-en.html` 单文件直出，无后端、无反代、无数据库、无框架、无 CDN。

## 环境与依赖（就这么多）

| 项 | 要求 | 说明 |
|---|---|---|
| Web 服务器 | nginx（推荐）或 Apache | 纯静态直服，**无后端进程、无反代** |
| 系统 | 任意 Linux（Debian/Ubuntu/宝塔均可） | 无 Python 运行时要求、无数据库、无 Node、无 Docker |
| 磁盘 | 约 28 MB | 站点本体 ~130 KB，`downloads/` 是绝对大头 |
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

## 页面结构（v0.2.276 版）

| 板块 | 内容 |
|---|---|
| Hero | 双 H1（主标题 + 副标题带 worldstip.com 外链）+ 两个 CTA + 4 项指标 |
| 能力 | 四支柱：国内全交通比选 / 住宿分房与预算 / 出境四灯 + 摩旅五门禁 / 摩旅路线与城市消费层 |
| 可信度 | stable / volatile / unknown 三档灯 + 9 处逻辑修正与 134 条反例说明 |
| 在线装 | 三个已上架市场入口卡（WorkBuddy / 千问 / SkillHub），各带**实测版本号**，另注百度搭子与专家入口只有包无公开页 |
| 下载 | 两个 tab（v0.2.276 五包 / v0.2.1 八包），卡片含解压体积、文件数、sha256 前 16 位 |
| 适配 | 五包适配矩阵表：目录结构 / frontmatter / 专属文件 / 解压体积 |
| 上手 | 装包 → 说需求 → 拿册子 |
| 验收 | 9 项实跑数据格子 |

## 下载包清单（v0.2.276）

资料库（个人空间）里这五包即权威版。下表每一行都用 `tools/build_pack.py` / `build_release_packs.py` / `build_qianwen_pack.py` / `build_expert_pack.py` 从本仓库源码重建后实算，重建产物与资料库下载件 **sha256 逐字节一致**。

| 平台 | 文件 | 文件数 | zip 体积 | 解压体积 | sha256 前 16 位 |
|---|---|---|---|---|---|
| WorkBuddy 技能 | `worldstip-travel-planner-v0.2.276-workbuddy.zip` | 95 | 1 020 489 B | 3 040 737 B | `2d6e7dc0c1e67e18` |
| WorkBuddy 专家 | `worldstip-travel-expert.zip` | 8 | 241 885 B | 250 430 B | `3915b126da175f0b` |
| 千问 AI | `worldstip-travel-planner-v0.2.276-qianwen.zip` | 85 | 970 331 B | 2 918 119 B | `0e152f6e7e15ca62` |
| 百度搭子 | `worldstip-travel-planner-v0.2.276-dumate.zip` | 132 | 1 598 736 B | 4 451 789 B | `43ce0d02377e75fa` |
| SkillHub | `worldstip-travel-planner-v0.2.276-skillhub.zip` | 132 | 1 598 553 B | 4 451 509 B | `af944723f77d9d42` |

v0.2.1 八包（`-fused` / `-expert-fused` / `-qianwen` / `-coze.skill` / `-baidu-dazi` / `-baidu-dazi-plugin` / `-baidu_databuilder` / `-skillhub`）同目录保留，页面切 tab 可见；v0.1.49 三包作为更早存档保留在 `downloads/` 但不展示。

## 打包规则（v0.2.276）

四平台包与专家包共用同一份文件清单（`build_pack.collect()`），固定时间戳 + 稳定排序 + LF 归一，**同一输入重建出字节一致的包**；`--verify` 逐条比对包内字节与源码，不一致即失败。

扩展名白名单：`.md` / `.json` / `.py` / `.svg` / `.txt`，另放行 `LICENSE`、`LICENCE`、`NOTICE` 三个无扩展名文件。MIT 要求许可证原文随分发，缺了会被平台判「内置文件缺失」，所以**不放行 `LICENSE` 那条规则已作废**。仍排除 `.gitignore`、`.skillhubignore`、`_tmp_*`、`__pycache__`、`tests/`、`releases/`、`expert/`、`HANDOFF.md`、`verify_backup.py`、`audit_upload_zip.py`。

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

## 各端上架实况（2026-11-08 复核）

| 平台 | 地址 | 页面标注版本 |
|---|---|---|
| WorkBuddy | `workbuddy.cn/share/asset?shareCode=1vcWCDMZzdZOCyznXg4EQg` | 页面为 SPA，正文无版本字段 |
| 千问 AI | `qianwenai.com/hub/skills/@user_gc9bq/worldstip-travel-planner` | v0.1.48（落后本地 v0.2.276） |
| SkillHub | `skillhub.cn/skills/indiv-mrdt/worldstip` | v1.0.0（独立版本序列） |
| 百度搭子 / 专家入口 | 无公开页 | — |

## 版本纪律

页脚固定「对外发布版本 = 本地开发包版本 = v0.2.276」。升级时：① 两页搜旧版本号（title / eyebrow / 页脚 / JSON-LD softwareVersion / 下载卡数据）；② `downloads/` 增补新包；③ `sitemap.xml` 的 lastmod 一并更新。

**注意**：不要对整页做版本号全局替换——`downloads/` 里的存档包名（如 `-v0.2.1-*`、`-v0.1.49-*`）必须保留原版本号，否则链接 404。

**当前同步状态（v0.2.276）**：
- 已同步：本站两页 + 13 个展示包（5 现行 + 8 存档）；GitHub 源码库 `Skill-WorldsTipAgent`。
- 实测数据：`doctor.py` **17/17**；`run_all.py` **17/17 套件 + 32/32 新增用例**；逻辑反例 **134/134**、融合专项 **45/45**、灰灯专项 **16/16**；脚本级断言 **1270 项**（geo 233 + moto 545 + enrich 119 + route_viz 77 + rv 64 + party 54 + guide_digest 41 + site_query 39 + guide_list 31 + render 26 + transit 23 + sync 18）；`audit_upload_zip.py` 6 包危险命中 **0**；最大 WorkBuddy 包解压 **2.90 MiB** 未越 3 MiB；包内 34 个脚本语法 0 错。

## 技术说明

- `index.html`：单文件约 82 KB，内联 CSS + 原生 JS，无框架、无 CDN、无外部图片。
- `index-en.html` 与中文页**共用同一段 CSS**（由 `_out/gen_en.py` 从中文页抽取样式段后拼装），改样式只需改中文页再重新生成。
- 滚动显现 / 数字滚动 / tab 切换 / `prefers-reduced-motion` 全降级。
- 下载卡用 flex 布局（`flex:1 1 270px`）：1440 视口下 5 张排成 **4+1**、8 张排成 **4+4**，末行不足时最后一张拉伸填满，不留半格；390 视口单列。
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
