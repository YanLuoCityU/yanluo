# Yan Luo Academic Homepage

这是 Yan Luo 的 GitHub Pages 学术个人主页仓库，基于 Jekyll / Academic Pages / Minimal Mistakes 架构定制。

网站地址：https://yanluocityu.github.io

## 项目内容

当前网站主要包含以下模块：

- **About Me**：个人简介、研究兴趣、教育背景和 Research Highlights。
- **Publications**：论文列表，按日期倒序展示，并显示作者、期刊、年份、影响因子和 JCR 分区。
- **Research Highlights**：首页重点论文卡片，链接到论文和媒体报道页面。
- **News / Media Coverage**：重点论文的英文和中文媒体报道。
- **Presentations**：口头报告和海报展示。
- **Teaching**：教学经历。
- **Honors**：奖项和奖学金。
- **Service**：学术服务和审稿经历。

## 核心目录

- `_config.yml`：全站基础配置，包括站点标题、作者信息、头像、社交链接、collection 配置等。
- `_pages/`：主要页面内容，例如首页、论文页、媒体报道页、教学、荣誉和服务页面。
- `_publications/`：论文 Markdown 文件，主要由 ORCID 同步脚本生成。
- `_data/`：导航、期刊指标、媒体报道等结构化数据。
- `_includes/`：可复用 Liquid 片段，例如论文条目的渲染模板。
- `_layouts/`：页面布局模板。
- `_sass/` 和 `assets/css/`：站点样式。
- `assets/js/`：站点 JavaScript。
- `images/`：头像、favicon、主题预览图等图片。
- `files/`：附件和 BibTeX 文件。
- `scripts/`：自动同步脚本。
- `.github/workflows/`：手动触发的数据同步工作流。
- `docs/WEBSITE_MODULES_AND_REQUIRED_FILES.md`：更细的模块与核心文件说明。

## 常见更新流程

### 更新个人简介、研究兴趣、教育背景

编辑：

```text
_pages/about.md
```

首页的 Research Highlights 也写在这个文件中。当前逻辑会从 `_publications/` 中按标题查找重点论文，并生成卡片。

### 更新顶部导航

编辑：

```text
_data/navigation.yml
```

删除条目会让该栏目从顶部导航消失，但不会删除对应页面。

### 更新作者信息、头像和社交链接

编辑：

```text
_config.yml
```

头像文件放在：

```text
images/
```

当前头像配置为：

```yaml
author:
  avatar: "profile-20260318.png"
```

修改 `_config.yml` 后，本地预览服务器通常需要重启。

### 更新 Publications

论文页面入口是：

```text
_pages/publications.html
```

单篇论文数据位于：

```text
_publications/
```

论文列表的单条渲染逻辑位于：

```text
_includes/archive-single.html
```

推荐流程是通过 ORCID 同步脚本自动更新：

```bash
python scripts/sync_orcid_publications.py --config _config.yml --output-dir _publications --files-dir files --allow-anonymous
```

GitHub Actions 中也有手动触发的同步流程：

```text
.github/workflows/sync_orcid_publications.yml
```

该 workflow 需要仓库 secrets：

```text
ORCID_CLIENT_ID
ORCID_CLIENT_SECRET
```

注意：ORCID 同步脚本会排除 conference abstract、conference paper、conference poster、conference presentation 等会议类型。会议相关内容应维护在 Presentations 模块，而不是 Publications。

### 更新期刊 IF / JCR

期刊指标数据位于：

```text
_data/journal_metrics.yml
```

论文条目会用 `venue` 字段匹配这个 YAML 文件中的期刊名。

可运行：

```bash
python scripts/sync_journal_metrics.py --timeout 10
```

该脚本需要 EasyScholar API key。GitHub Actions 中的手动同步流程是：

```text
.github/workflows/sync_journal_metrics.yml
```

需要仓库 secret：

```text
EASYSCHOLAR_API_KEY
```

### 更新 News / Media Coverage

页面文件：

```text
_pages/research-highlights-media.html
```

自动抓取数据：

```text
_data/research_highlights_media.json
```

手工修正和精选报道：

```text
_data/research_highlights_media_manual.yml
```

推荐维护方式：

1. 如需刷新 Nature metrics 基础数据，运行：

   ```bash
   python scripts/sync_research_highlights_media.py
   ```

2. 如需调整摘要、中文报道、报道排序或修正抓取结果，优先编辑：

   ```text
   _data/research_highlights_media_manual.yml
   ```

页面会先读取自动 JSON，再用手工 YAML 覆盖对应条目。

### 更新 Presentations、Teaching、Honors、Service

这些页面目前都是手写页面，不依赖旧的 collection。

对应文件：

```text
_pages/talks.html
_pages/teaching.md
_pages/honors.md
_pages/service.md
```

## 本地预览

安装 Ruby、Bundler 和 Node.js 后，在仓库根目录运行：

```bash
bundle install
bundle exec jekyll serve -l -H localhost
```

打开：

```text
http://localhost:4000
```

如果修改了 `_config.yml`，需要停止并重启 Jekyll 服务。

## Docker 预览

如果本机不想配置 Ruby 环境，可以使用 Docker：

```bash
docker compose up
```

打开：

```text
http://localhost:4000
```

在 Linux 或 WSL 环境下，如遇权限问题，可先运行：

```bash
chmod -R 777 .
```

## JavaScript 构建

`package.json` 中保留了前端脚本构建命令。若修改了 `assets/js/_main.js` 或相关插件，需要重新生成压缩文件：

```bash
npm install
npm run build:js
```

生成目标：

```text
assets/js/main.min.js
```

## 部署

这是 GitHub Pages 用户站点仓库。提交并推送到 GitHub 后，GitHub Pages 会构建并发布网站。

提交前建议检查：

- 页面内容是否在正确文件中更新。
- `_config.yml` 修改后是否本地重启并预览过。
- Publications 是否来自 ORCID 同步，避免手工改动被下次同步覆盖。
- 媒体报道修正是否优先写入 `_data/research_highlights_media_manual.yml`。
- 如修改样式或模板，是否本地预览了首页、Publications 和移动端布局。

## 维护速查

- 改首页内容：`_pages/about.md`
- 改论文：`_publications/` 或 `scripts/sync_orcid_publications.py`
- 改论文展示格式：`_includes/archive-single.html`
- 改期刊指标：`_data/journal_metrics.yml` 或 `scripts/sync_journal_metrics.py`
- 改媒体报道：优先 `_data/research_highlights_media_manual.yml`
- 改导航：`_data/navigation.yml`
- 改站点身份、头像、作者信息：`_config.yml`
- 改样式：`_sass/` 或 `assets/css/`
- 改附件：`files/`
- 改图片：`images/`
