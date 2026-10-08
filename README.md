# 大学手记

基于 Hugo + PaperMod 的个人博客。暖白背景、文字为主，支持手机、明暗主题、正文搜索、归档、RSS 和摄影索引。

- 网站：https://agit-internet.github.io/
- 仓库：https://github.com/AGIT-Internet/agit-internet.github.io
- 在线编辑：https://github.dev/AGIT-Internet/agit-internet.github.io
- 查看部署：https://github.com/AGIT-Internet/agit-internet.github.io/actions

## 最方便的在线更新方式

1. 在仓库打开 `content/posts`，进入某篇文章，点击编辑按钮；或 Add file → Create new file 新建文章。
2. 新文件建议用 `content/posts/2026-10-08-my-day.md` 这样的英文文件名。
3. 复制下面的文章头，写正文，将 `draft` 改为 `false`。
4. Commit changes 到 `main`，部署成功后网站自动更新。
5. 照片使用 Add file → Upload files 上传。编辑器中选择 Markdown Preview 可以看正文效果；完整网站效果以部署后的页面为准。
6. 同时使用本地和在线编辑时，本地写作前先 `git pull --rebase`，避免两个版本冲突。

```markdown
---
title: "今天的记录"
date: 2026-10-08
slug: "my-day-2026-10-08"
draft: false
categories: ["日常随笔"]
tags: ["生活"]
description: "一句简短的摘要"
ShowToc: false
---

在这里开始写。
```

四种建议分类：日常随笔、大学总结、技术文章、摄影。可以自行添加分类和标签；同一天可写多篇，保持文件名和 slug 唯一。

## 本地写作

首次在新电脑上获取项目：

```powershell
git clone --recurse-submodules https://github.com/AGIT-Internet/agit-internet.github.io.git
cd agit-internet.github.io
winget install -e --id Hugo.Hugo.Extended
hugo server -D
```

在浏览器打开 http://localhost:1313/。安装后如果找不到 hugo，请重新打开终端。

创建不同类型的草稿：

```powershell
hugo new content --kind diary posts/my-day.md
hugo new content --kind summary posts/semester-summary.md
hugo new content --kind tech posts/my-tech-note.md
hugo new content --kind photo posts/my-photo-story
```

发布前把文章头的 `draft: true` 改为 `false`，日期应不晚于当前时间。

```powershell
hugo --gc --minify
git add .
git commit -m "docs: add journal entry"
git push
```

`content/posts/example-*.md` 和 `content/posts/example-photo/` 是示例草稿，不会正式发布。

## 摄影

每组照片放在同一个文章文件夹中：

```text
content/posts/campus-autumn/
  index.md
  cover.jpg
  library.jpg
```

`index.md`：

```markdown
---
title: "校园的秋天"
date: 2026-10-08
draft: false
categories: ["摄影"]
ShowToc: false
cover:
  image: "cover.jpg"
  alt: "秋日校园"
  relative: true
---

傍晚去校园里走了一圈。

![图书馆前的光影](library.jpg)
```

带有“摄影”分类的文章会自动进入摄影页面。文章封面会出现在摄影索引中；索引和正文支持手机浏览。

建议上传经过压缩的 JPG 或 WebP，单张尽量小于 1 MB。摄影模板中的 SVG 只是草稿占位图，请替换为自己的照片。替换时同步修改文章头和正文中的文件名。

## 评论

仓库已开启 Discussions，giscus 已安装并启用。评论模板和仓库标识位于 `hugo.yaml` 与 `layouts/_partials/comments.html`。

以后迁移到其他仓库时，重新配置 giscus：
1. 打开 https://github.com/apps/giscus/installations/new 。
2. 选择 AGIT-Internet，仅授权 agit-internet.github.io。
3. 将 `params.comments` 和 `params.giscus.enabled` 都设为 `true`。
4. 使用 https://giscus.app/zh-CN 获取新仓库与分类标识，更新配置后提交。

评论以文章路径关联到 Announcements 分类。发布后尽量保持 slug 不变，避免改变原评论关联。访客发表评论需要登录 GitHub。评论主题会随博客明暗切换。

## 搜索与订阅

- 搜索地址：https://agit-internet.github.io/search/
- RSS 地址：https://agit-internet.github.io/index.xml
- 搜索包含标题、摘要和正文，是浏览器内的文字匹配，适合个人博客规模。
- 仅发布正式文章；示例草稿不进入线上搜索和订阅。
- 这是公开博客。未发布的草稿虽然不会显示在网站，公开仓库中的源文件仍可被读取；私人记录请留在自己的本地目录。

## 修改名字与外观

- `hugo.yaml`：网站标题、作者、首页介绍、菜单、评论开关。
- `content/about.md`：关于页面。
- `assets/css/extended/journal.css`：背景色、字体、间距。
- `layouts/_partials/home_info.html`：首页介绍。
- `static/favicon.svg`：浏览器标签图标。

作者名暂使用当前 Git 设置中的 GalaxyVortex，可随时修改。

## 更新主题

主题以 Git 子模块固定在已验证的版本。不要直接修改 `themes/PaperMod`；自定义文件放在项目自己的 `layouts` 和 `assets` 中。

```powershell
git submodule update --remote themes/PaperMod
hugo --gc --minify
hugo server -D
```

预览确认后，提交新的子模块版本。Hugo 构建版本固定为 0.167.0；如升级本地版本，按需同步修改 `.github/workflows/hugo.yaml`。

## 自定义域名与统计

以后可在仓库 Settings → Pages 添加域名，再修改 `hugo.yaml` 的 `baseURL`。当前 Actions 工作流读取 Pages 设置来生成正确链接。

访问统计可将 Cloudflare Web Analytics 提供的脚本放入 `layouts/_partials/extend_footer.html`，无需改变 GitHub Pages 托管。

## 部署失败时

- 到 Actions 查看失败步骤；正式部署需要源文件和主题子模块都已提交。
- 确认 Settings → Pages → Source 为 GitHub Actions。
- 搜索无结果时先确认文章 `draft: false`、日期已到，再检查 /index.json 是否可打开。
- 新电脑主题缺失时执行 `git submodule update --init --recursive`。

主题：https://github.com/adityatelange/hugo-PaperMod
Hugo：https://gohugo.io/
