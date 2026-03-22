# BXI Robotics Wiki (开发者文档中心)

[English Version Below](#english-version)

欢迎来到 BXI Robotics 开发者文档中心代码仓库。本文档库基于 **MkDocs Material** 主题以及多款强大的扩展插件构建，支持中英双语，并配备了全自动部署 (CI/CD) 到 1Panel 服务器的流水线。

---

## 🛠️ 文档工作流：文章是如何生效上线云端的？

由于我们为您配置了 GitHub Actions 自动化脚本，本站的更新流程**完全不需要开发者在电脑上进行打包编译**。
只需以下两步纯傻瓜式操作：

1. 在本地使用编辑器创建或修饰 Markdown 文档内容。
2. 执行 `git push` 把您的本地修改推送到 `master` 主分支。

**底层全自动原理**：推送后，GitHub 会自动把纯文本代码的目录（包含 `docs/` 和 `mkdocs.yml`）同步到 BXI 远程服务器挂载点。随后发起指令远程执行 `docker compose restart` 重启云端 MkDocs 容器。容器在开机瞬间会自动拉取 `requirements.txt` 里列出的最新插件资源，并在内存中实时渲染新更新的网页。

---

## 📂 页面路径与双语结构映射

我们使用严格隔离的双语文件夹方案。所有的图片等静态共享资源，存放在独立的公共池中不分家。

```text
docs/
├── assets/          <-- ★ 全站共享资源库（图片、视频、下载附件）
│   ├── elf3/
│   └── ...
├── zh/              <-- 🇨🇳 中文主站文档区
│   ├── elf3/
│   │   ├── index.md         <-- 该类的首页总览
│   │   └── quick_start.md   <-- 任意自建的一篇文档
│   └── index.md             <-- 整个网站的首页
└── en/              <-- 🇬🇧 英文主站文档区
    ├── elf3/
    └── index.md
```

### 1. 撰写文档的格式：Markdown 约束
所有新资料都请使用标准的 Markdown 语法。
- **共享图片插入**：**禁止存两份相同的图片**！无论您在修改中文还是英文版，请将图纸照片等媒体全放进全局唯一的 `docs/assets/(对于的产品型号)/` 文件夹底下。并利用相对路径退法在文章中调用：
  `![功能说明](../../assets/elf3/demo.jpg)`

### 2. 目录栏目创建与左侧菜单排序
我们的网站开启了自动化插件引擎，所以您**无需**手动干预 `mkdocs.yml` 去维护菜单树。页面的左侧导航菜单条是由 `awesome-pages` 引擎**自动扫描您的文件夹系统架构**实时变幻出来的！
- **新建栏目**：如果您要上线一款叫 `dog1` 的新机器部件，直接暴力在 `docs/zh/` 和 `docs/en/` 下各建一个 `dog1` 的子文件夹丢进去即可。
- **分类主页必须存在**：您新建的子文件夹里一定要有一个叫 `index.md` 的打底文件，用作该栏目的落地汇总入口篇。
- **自定义文章的排序**：如果您觉得机器由于字母顺序倒排出来的文章，把“高级开发”错误顶在了“快速入门前面，那该怎么办？只需在引起错乱的这个当前文件夹（比如 `elf3` 里）新建一个无扩展名的黑户文本文件，取名叫做 `.pages`，并在里面这样按顺序写代码：
  ```yaml
  nav:
    - index.md
- **自定义文章的排序**：如果您觉得机器由于字母顺序倒排出来的文章，把“高级开发”错误顶在了“快速入门前面，那该怎么办？只需在引起错乱的这个当前文件夹（比如 `docs/zh` 或 `elf3` 里）新建一个无扩展名的黑户文本文件，取名叫做 `.pages`，并在里面按顺序写上下列代码，最后加个统配符 `- ...` 兜底：
  ```yaml
  nav:
    - index.md
    - quick_start.md
    - ...
  ```
- **自定义顶部多语言栏目名称 **：系统默认直接读取英文字母文件夹名作为顶部菜单名。如果我们想给一个大栏目（如 `joint_module`）重命名为“关节模组”，**最无脑简单的做法就是在该子文件夹里新建一个 `.pages` 文件，只写一行代码 `title: 您的中文名`**：
  ```yaml
  title: 关节模组
  ```
- **单篇文章页面的自定义命名 (给首页重命名)**：如果您想让 `index.md` 这种单个文章不要显示为冷冰冰的英文字母，直接在**文章文件的最顶部**加上一个 YAML Frontmatter 头即可：
  ```yaml
  ---
  title: 首页
  ---
  ```

### 3. 文件命名强制规范
1. **文件夹名 / 物理文件名**：一律只允许使用**小写基础英文字母或数字**！如果长难句词组，只能用下划线 `_` 或中划线 `-` 连接（例如 `joint_module` / `advanced_quick_start.md`）。**绝不可使用中文或大空格命名真实文件实体**，否则网页 URL 会陷入系统级乱码和崩溃报错怪圈！
2. **网页左侧菜单读取的真正标题**：虽然物理文件名是洋字母，但在网页里左侧导航栏树真正长出来的标题名字是很聪明的：MkDocs 会自动读取您的 Markdown 内容里面的第一个含有 `# 的大标题`（例如 `# 关节模组说明书`），去当作侧边菜单的名字。如果您不想抽调正文大标题字面，可以写在 `.md` 文件第一行的控制头上（Frontmatter）：
   ```yaml
   ---
   title: 我是展示给外面侧边菜单看的名字，和正文无关
   ---
   ```


---
<br/>
<a name="english-version"></a>

# BXI Robotics Wiki (Developer Documentation Repository)

Welcome to the central BXI Robotics Wiki code repository. This documentation architecture is powered by the **MkDocs Material** theme supplemented by advanced extensions, seamlessly driving multi-language environments (Chinese and English) and deployed entirely autonomously to 1Panel containers via GitHub Actions.

---

## 🛠️ Documentation Workflow: How Is Code Brought to Live Stages?

Thanks to the automated GitHub CI pipelines pre-configured under `.github/workflows/deploy.yml`, authors will **never** need to execute complex `mkdocs build` protocols explicitly on local rigs. Editing proceeds painlessly through two main steps:

1. Maintain or draft your new Markdown files.
2. Form a commit and enact `git push` directly onto `master` or `main`.

**Automated Deep Mechanics**: As a push is intercepted, GitHub synchronizes non-compiled, bare-bone references (`docs/`, `mkdocs.yml`, `requirements.txt`) directly via SCP onto the 1Panel volume mount paths. Post synchronization, an overarching SSH command triggers a restart logic for the active MkDocs docker suite running live in the data-center nodes. Upon spin-up, the container recursively checks into our `requirements` map dynamically and caches extensions securely before mounting an agile layout in memory. 

---

## 📂 Page Routing & Bilingual File Tree Structure

As our engine governs translated sites context, documents are strictly silo-ed beneath the respective language root node folders preventing indexing cross-contamination. Meanwhile, binary/media items leverage a shared overarching parent vault for re-usability. 

```text
docs/
├── assets/          <-- ★ Global Shared Content (Images, Videos, CADs)
│   ├── elf3/
│   └── ...
├── zh/              <-- 🇨🇳 Main Chinese Translation Source
│   ├── elf3/
│   │   ├── index.md         <-- Fallback root gateway 
│   │   └── quick_start.md   <-- Supplemental sub-sheet
│   └── index.md
└── en/              <-- 🇬🇧 Main English Translation Source
    ├── elf3/
    └── index.md
```

### 1. Markdown Core Standards 
Contributions must follow the fundamental Markdown guidelines.
- **Global Image Referencing**: **Never duplicate matching media blobs twice into separated directories**. Route the source file into the unifying `docs/assets/[Sub-Series_Name]/` library and invoke the relative exit path to pull the asset onto current domains reliably:
  `![Descriptive Tagging](../../assets/elf3/demo.jpg)`

### 2. Spawning New Index Categories & Custom Filtering
The repository does **not** rely on authors performing manual updates towards maintaining the nested YAML `nav:` branch maps inside the settings configuration. Through utilizing the `awesome-pages` framework mechanism, MkDocs detects your branch layout context inherently!
- **Setting Up New Directory Routes**: Deploying a completely separate structural layer means just appending a folder matching the new product unit names, under `docs/zh` and inversely mapped inside `docs/en`, such as `dog1`.
- **Primary Sub-Link Requirement**: Keep in mind that inside any initialized content layer branch, an index root must be defined (thus ensuring you drop an `index.md` acting as the welcoming entry module).
- **Overriding Alphabetic Sort Constraints**: The automated crawler processes contents and organizes trees following strict alphabetic indexing rules. For circumstances requiring custom sorting (Ex: Pushing the "Quick Guide" link above the "Advanced Breakdown"), initialize a dotfile named `.pages` strictly inside the acting problematic directory carrying sorting overrides syntax:
  ```yaml
  nav:
    - index.md
    - quick_start.md
    - ...
  ```
- **Customizing Top-Level Tab Names**: By default, the plugin extracts raw physical folder strings (e.g., `joint_module`) to render the top root tabs. To forcefully map proper capitalized names or localized translations, initialize a `.pages` file within that specific subfolder defining solely the title metadata:
  ```yaml
  title: Joint Module
  ```
- **File-Level Title Overrides (Homepage Injection)**: To overwrite the display name of raw Markdown page entities (like swapping "index" text in the menu to read "Home"), seamlessly inject explicit YAML Frontmatter atop the exact file itself `index.md`:
  ```yaml
  ---
  title: Home
  ---
  ```

### 3. Naming Convention Formalities & URL Generations
1. **File / Folder Entities**: Restrict standard file naming mechanisms uniquely to lowercase alphabet digits merged consistently by an underscore `_` or dash `-` (`joint_module`, `quick_start.md`). Deployments integrating UTF8-encoded Chinese script variants natively inside raw filesystem entities generate unreadable URL parameters breaking active site structures inevitably!
2. **Dynamic Sidebar Headers Lookup Parsing**: Although the base filename inherits English characters dynamically, the interface abstracts these paths smartly. The sidebar menu actively pulls the first `# H1 Heading` literal mapping from internal markup document syntax. Should it be required to override external menu aliases distinctly from heading representations, utilize the top-line YAML block parameter insertion procedure (`frontmatter`):
   ```yaml
   ---
   title: Overridden Short Name For Explicit Sidebar Rendering
   ---
   ```
