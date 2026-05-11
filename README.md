# YF's Notes 个人网站维护说明

这是一个使用 MkDocs 搭建的个人笔记网站。网站内容主要写在 `docs/` 目录中，网站结构和导航主要由 `mkdocs.yml` 控制，推送到 GitHub 后会通过 GitHub Actions 自动部署到 GitHub Pages。

## 项目中各文件夹介绍

### `.github/`

存放 GitHub 相关配置。

当前项目中主要使用的是 `.github/workflows/deploy.yml`，它定义了自动部署流程：当代码推送到 `main` 分支后，GitHub Actions 会安装 MkDocs 和 Material 主题，然后执行 `mkdocs gh-deploy --force`，把网站部署到 GitHub Pages。

### `docs/`

网站的 Markdown 源文件目录，也是日常写笔记时最主要操作的地方。

MkDocs 默认会把 `docs/` 作为网站内容根目录。比如：

- `docs/index.md` 对应网站首页。
- `docs/健身/0.训练计划.md` 对应“健身”分组下的“入门”页面。
- `docs/健身/1.健身记录.md` 对应“健身”分组下的“记录”页面。
- `docs/健身/2.饮食计划.md` 对应“健身”分组下的“饮食”页面。

如果 Markdown 中需要引用图片，建议把图片放在同一个主题文件夹下，例如 `docs/健身/哑铃卧推.png`，然后在 Markdown 里使用相对路径引用：

```markdown
![哑铃卧推](哑铃卧推.png)
```

### `docs/.obsidian/`

这是 Obsidian 的配置目录。如果你用 Obsidian 打开 `docs/` 作为知识库，这里面会保存 Obsidian 的主题、插件、工作区、快捷键等配置。

它不直接决定网站内容，但会影响你在 Obsidian 中编辑笔记时的体验。

### `site/`

MkDocs 构建后生成的静态网站目录。

一般情况下不需要手动编辑 `site/` 里的文件，因为它们是由 `docs/` 和 `mkdocs.yml` 自动生成的。如果你修改了 `site/`，下次重新构建网站时这些修改可能会被覆盖。

### `.git/`

Git 仓库内部目录，用来保存版本记录、分支、提交等信息。

不要手动修改这个目录。

### `mkdocs.yml`

MkDocs 的核心配置文件，控制网站名称、主题、导航结构、Markdown 扩展、额外 JavaScript 等。

当前项目中比较重要的配置包括：

```yaml
site_name: YF's Notes
theme:
  name: material
  language: zh

nav:
  - Home: index.md
  - 健身:
    - 入门: 健身/0.训练计划.md
    - 记录: 健身/1.健身记录.md
    - 饮食: 健身/2.饮食计划.md
```

其中 `nav` 决定网站顶部或侧边栏中显示哪些页面，以及它们属于哪个分组。

### `.gitignore`

Git 忽略规则文件，用来指定哪些文件不需要提交到仓库。

## 网站的管理方法

### 本地预览网站

在项目根目录运行：

```powershell
mkdocs serve
```

然后在浏览器打开终端中显示的本地地址，通常是：

```text
http://127.0.0.1:8000/
```

本地预览时，修改 `docs/` 里的 Markdown 文件后，页面通常会自动刷新。

如果本机还没有安装 MkDocs，可以先安装：

```powershell
pip install mkdocs mkdocs-material
```

### 添加 Markdown 文件并同步到个人网站

添加一篇新笔记通常分为四步：

1. 在 `docs/` 下选择合适的文件夹。

   例如要添加健身相关笔记，可以放到：

   ```text
   docs/健身/
   ```

2. 新建 `.md` 文件。

   例如：

   ```text
   docs/健身/3.动作要点.md
   ```

3. 在 `mkdocs.yml` 的 `nav` 中加入这个页面。

   例如把新页面加入“健身”分组：

   ```yaml
   nav:
     - Home: index.md
     - 健身:
       - 入门: 健身/0.训练计划.md
       - 记录: 健身/1.健身记录.md
       - 饮食: 健身/2.饮食计划.md
       - 动作要点: 健身/3.动作要点.md
   ```

4. 提交并推送到 GitHub。

   ```powershell
   git add docs mkdocs.yml
   git commit -m "Add fitness note"
   git push
   ```

推送到 `main` 分支后，`.github/workflows/deploy.yml` 会自动触发部署。部署完成后，个人网站上就会出现这篇新笔记。

### 添加新的分组

如果想在网站中添加一个新的一级分组，比如“编程”，可以按下面的方式操作。

1. 在 `docs/` 下新建文件夹：

   ```text
   docs/编程/
   ```

2. 在新文件夹里添加 Markdown 文件：

   ```text
   docs/编程/0.Python基础.md
   ```

3. 修改 `mkdocs.yml`，在 `nav` 中新增分组：

   ```yaml
   nav:
     - Home: index.md
     - 健身:
       - 入门: 健身/0.训练计划.md
       - 记录: 健身/1.健身记录.md
       - 饮食: 健身/2.饮食计划.md
     - 编程:
       - Python基础: 编程/0.Python基础.md
   ```

4. 本地预览确认无误：

   ```powershell
   mkdocs serve
   ```

5. 提交并推送：

   ```powershell
   git add docs mkdocs.yml
   git commit -m "Add programming section"
   git push
   ```

### 修改已有页面

如果只是修改已有笔记，直接编辑 `docs/` 中对应的 `.md` 文件即可。

例如修改健身记录：

```text
docs/健身/1.健身记录.md
```

修改完成后可以本地预览，再提交推送：

```powershell
mkdocs serve
git add docs
git commit -m "Update fitness record"
git push
```

### 管理图片和附件

建议把图片放在使用它的 Markdown 文件附近，便于整理和迁移。

例如：

```text
docs/健身/动作要点.md
docs/健身/哑铃卧推.png
```

在 `动作要点.md` 中引用：

```markdown
![哑铃卧推](哑铃卧推.png)
```

如果图片放在子文件夹中，例如：

```text
docs/健身/images/哑铃卧推.png
```

则引用方式为：

```markdown
![哑铃卧推](images/哑铃卧推.png)
```

## 实操案例：往个人网站中添加一篇笔记的全流程

下面以“添加一篇健身笔记：肩部训练要点”为例。

### 第 1 步：确定笔记所属分组

这篇笔记属于健身内容，所以放到：

```text
docs/健身/
```

### 第 2 步：新建 Markdown 文件

新建文件：

```text
docs/健身/3.肩部训练要点.md
```

可以写入类似内容：

```markdown
# 肩部训练要点

## 训练目标

- 增强肩部稳定性
- 提升三角肌前束、中束、后束的训练质量
- 改善推举类动作的发力感
```

### 第 3 步：如果有图片，把图片放到同一目录

例如把图片放到：

```text
docs/健身/坐姿哑铃推肩.png
docs/健身/哑铃侧平举.png
docs/健身/面拉练肩后束.png
```

然后在 Markdown 中引用：

```markdown
![坐姿哑铃推肩](坐姿哑铃推肩.png)
![哑铃侧平举](哑铃侧平举.png)
![面拉练肩后束](面拉练肩后束.png)
```

### 第 4 步：把新笔记加入网站导航

打开 `mkdocs.yml`，找到：

```yaml
nav:
  - Home: index.md
  - 健身:
    - 入门: 健身/0.训练计划.md
    - 记录: 健身/1.健身记录.md
    - 饮食: 健身/2.饮食计划.md
```

添加一行：

```yaml
    - 肩部训练要点: 健身/3.肩部训练要点.md
```

修改后变成：

```yaml
nav:
  - Home: index.md
  - 健身:
    - 入门: 健身/0.训练计划.md
    - 记录: 健身/1.健身记录.md
    - 饮食: 健身/2.饮食计划.md
    - 肩部训练要点: 健身/3.肩部训练要点.md
```

### 第 5 步：提交修改

确认无误后执行：

```powershell
git status
git add docs mkdocs.yml
git commit -m "Add shoulder training note"
```

### 第 6 步：推送到 GitHub 并自动部署

执行：

```powershell
git push
```

推送到 `main` 分支后，GitHub Actions 会自动运行 `.github/workflows/deploy.yml` 中的部署流程。

可以在 GitHub 仓库页面的 `Actions` 标签页查看部署状态。部署成功后，刷新个人网站，就能看到新增的笔记。


