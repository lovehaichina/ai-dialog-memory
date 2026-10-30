# 2026-01-15-博客迁移与笔记工具选型

## 主题一：静态博客从 Hexo 迁移到 Astro

### 任务目标
把现有 Hexo 博客迁移到 Astro，提升构建速度和页面加载性能。

### 关键决策
- 选 Astro 而非 Next.js：内容站不需要 React 水合，Astro 输出纯静态 HTML
- 主题选用 Fuwari 模板，简约风适合技术笔记
- 评论系统用 Giscus（GitHub Discussions 驱动），不挂第三方评论

### 执行结果
迁移完成，127 篇文章全部导入，Lighthouse 性能分从 62 提升到 94。

### 涉及文件
- `~/blog/astro-site/`（新项目源码）
- `~/blog/hexo-backup/`（旧站备份）

### 图片附件
- 见对话第 5 轮截图（Lighthouse 前后对比）

### 待办
- 配置 RSS 订阅和 sitemap
- 提交搜索引擎收录

### 可复用经验
- 迁移博客先备份旧站 `source/` 和 `_config.yml`，不要直接删
- Hexo 标签语法 `{% %}` 在 Astro MDX 里会报错，迁移时要批量替换

### 新踩坑
- if-then：迁移 Markdown 文件时→图片相对路径 `../images/` 在 Astro 目录结构下断链→必须先统一改成 `@/assets/` 别名或内联 public/ 目录

### 来源引用
- Astro 官方迁移指南
- Fuwari 模板 README

---

## 主题二：本地笔记工具选型

### 任务目标
在 Obsidian、Logseq、思源笔记之间选一个作为长期笔记工具。

### 关键决策
- 选 Obsidian：本地 Markdown 文件、插件生态最丰富、数据不锁定
- 排除 Logseq：大纲式笔记不适合长篇文章
- 排除思源笔记：闭源数据格式，迁移成本高

### 执行结果
Obsidian 已配置核心插件（模板、日记、双链），笔记目录同步到 Git 仓库。

### 涉及文件
- `~/notes/`（笔记目录，已 Git 初始化）

### 图片附件
（无）

### 待办
- 搭建多设备同步方案（建议 Syncthing）
- 配置备份到云端

### 可复用经验
- 笔记工具选型第一原则：数据格式必须是开放的纯文本（Markdown），否则锁定后迁移痛苦

### 新踩坑
（无）

### 来源引用
- 各工具官方文档
