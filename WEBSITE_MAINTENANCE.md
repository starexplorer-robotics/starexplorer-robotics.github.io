# 网站维护手册

这份文档面向队内同学，目标是让大家在 **不会 HTML 也能维护网站** 的前提下，比较稳地更新内容。

当前站点基于 Jekyll，但日常维护大部分时候只需要改：

- Markdown 文档
- YAML 数据文件
- 少量页面文案

公开站点目前的原则是：

- 重点展示队伍形象、机器人实物、基础硬件配置、比赛准备情况
- **不主动公开具体算法、模型、规划方法和内部实现细节**
- 如果之后要恢复技术细节，先确认是否真的适合放在官网

## 1. 仓库结构

最常用的目录和文件如下：

```text
_data/
  team.yml           队伍信息、简介、成员
  hardware.yml       硬件模块、参数表、设备清单
  competition.yml    比赛重点、实验进展、路线图
  resources.yml      资源链接
  highlights.yml     首页卡片
  workflows.yml      系统页/首页的概览卡片

assets/
  css/main.css       整站样式
  js/site.js         导航菜单等少量交互
  images/            网页图片

index.html           首页
team.html            队伍页
systems.html         系统页
competition.html     比赛页
resources.html       资源页

README.md            简介
WEBSITE_MAINTENANCE.md  这份维护手册
```

## 2. 平时最常改哪些文件

### 改队伍介绍

改这里：

```text
_data/team.yml
```

这里控制的内容包括：

- 队伍简介
- 比赛名称
- quick facts
- 成员与分工
- 对外展示的基本原则文案

### 改硬件配置

改这里：

```text
_data/hardware.yml
```

这里控制的内容包括：

- 系统页里的硬件卡片
- 机器人参数表
- 机械臂参数表
- 补充硬件清单
- 系统页顶部的硬件快照

### 改比赛状态和准备情况

改这里：

```text
_data/competition.yml
```

这里控制的内容包括：

- 比赛页里的 priorities
- 实验进展
- 路线图
- 部署/运输信息

### 改首页卡片

改这里：

```text
_data/highlights.yml
```

### 改资源页链接

改这里：

```text
_data/resources.yml
```

## 3. 哪些文件是模板，不要轻易大改

下面这些文件是页面模板层，建议只有在明确知道自己要改什么时再动：

- `index.html`
- `team.html`
- `systems.html`
- `competition.html`
- `resources.html`
- `_layouts/default.html`
- `_includes/site-header.html`
- `_includes/site-footer.html`

简单理解：

- `_data/*.yml` 是“内容”
- `*.html` 是“排版”
- `assets/css/main.css` 是“外观”

如果只是改文字、成员、参数、链接，优先改 `_data`，不要先改 HTML。

## 4. 当前图片怎么维护

网页图片统一放在：

```text
assets/images/
```

目前官网里使用的机器人实拍图是：

```text
assets/images/robot-photo.jpg
```

它现在被这些页面使用：

- `index.html`
- `systems.html`
- `resources.html`

### 更换图片

1. 把新图放到 `assets/images/`
2. 文件名尽量简单，例如：

```text
robot-photo-2026.jpg
```

3. 再到对应页面里把图片路径改掉，例如：

```html
<img src="{{ '/assets/images/robot-photo.jpg' | relative_url }}" alt="...">
```

### 图片大小建议

网页图片不要太大，否则加载会慢。

建议：

- 宽度控制在 1600 到 2000 像素左右
- 文件尽量压到 1MB 左右

macOS 可以用 `sips` 压图：

```bash
sips -Z 1800 原图.jpg --out 新图.jpg
```

## 5. 如果想继续从 PDF 里取图

如果 TDP 里有合适图片，可以先导出，再放到 `assets/images/`。

当前仓库里已经演示过这件事：从 PDF 导出了图片，再生成网页用图。

原则上：

- PDF 里地图、算法示意图不要随便放首页
- 机器人实拍图、赛场照片、设备照片更适合官网

## 6. 如何新增一个成员

打开：

```text
_data/team.yml
```

找到 `members:`，按下面格式新增一项：

```yml
- name: New Member
  role: Hardware Integration
```

注意：

- 缩进用空格，不要用 tab
- `-` 前面不要多缩进

## 7. 如何修改硬件参数表

打开：

```text
_data/hardware.yml
```

你会看到类似：

```yml
tables:
  robot:
    rows:
      - attribute: Name
        value: Go2W
```

只改 `value` 就行。

如果要新增一行，也照这个结构继续加：

```yml
      - attribute: New Item
        value: New Value
```

## 8. 如何新增一个页面

最简单的做法：

1. 复制一个现有页面，比如 `team.html`
2. 改顶部 front matter

```yml
---
title: New Page
permalink: /new-page/
description: ...
body_class: page-new
---
```

3. 把内容改成你要的
4. 再去这里加导航：

```text
_data/navigation.yml
```

加一项：

```yml
- title: New Page
  url: /new-page/
```

## 9. 如何改样式

样式全部集中在：

```text
assets/css/main.css
```

常见修改：

- 颜色变量：在文件最上面的 `:root`
- 卡片样式：`.card`、`.panel`、`.media-card`
- 首页大图：`.media-card--hero`
- 表格样式：`.data-table`
- 手机端布局：文件底部 `@media` 部分

如果只是想改颜色，优先改这些变量：

```css
:root {
  --bg: ...;
  --surface: ...;
  --text: ...;
  --accent: ...;
}
```

## 10. 本地预览方式

### 最省事的方法

不在本地预览，直接改完 push，让 GitHub Pages 自动构建。

优点：

- 不需要配本地环境
- 适合只是改文案和数据

缺点：

- 要 push 以后才能看到最终效果

### 本地预览方法

网站是 Jekyll，所以本地预览需要 Ruby 环境。

这台 Mac 当前已经有 Homebrew Ruby，只是终端默认还在用系统 Ruby。

推荐做法：

1. 让终端优先使用 Homebrew Ruby

```bash
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
ruby -v
```

2. 进入仓库安装依赖

```bash
bundle install
```

3. 启动本地预览

```bash
bundle exec jekyll serve
```

4. 打开浏览器访问

```text
http://127.0.0.1:4000
```

### 如果本地预览失败

优先检查：

- `ruby -v` 不是系统的 `2.6`
- `which ruby` 指向的不是 `/usr/bin/ruby`
- 有没有安装 Xcode Command Line Tools

安装命令行工具：

```bash
xcode-select --install
```

## 11. 编辑 YAML 时最容易犯的错

### 缩进错

YAML 很怕缩进错。统一用 2 个空格。

### 冒号后面少空格

错误：

```yml
title:Hello
```

正确：

```yml
title: Hello
```

### 列表项缩进不一致

错误：

```yml
members:
 - name: A
   role: X
```

正确：

```yml
members:
  - name: A
    role: X
```

## 12. 公开站点内容边界

当前官网建议只放这些内容：

- 队伍简介
- 机器人实拍图
- 基础硬件配置
- 比赛准备情况
- 成员和分工
- TDP / 资源链接

不建议直接放到官网首页或系统页的内容：

- 具体算法名
- 具体模型名
- 详细规划流程
- 内部实现细节
- 太学术的图表和调参信息

如果以后要发技术博客，可以单独新开页面，不要直接堆在官网主展示页面里。

## 13. 推荐维护流程

每次更新建议按这个顺序：

1. 先改 `_data/*.yml`
2. 如果需要，再改对应页面 HTML
3. 本地预览或直接推送到 GitHub
4. 打开首页、系统页、资源页看一遍
5. 检查手机端是否正常换行

## 14. 最后一个建议

如果以后你们希望进一步降低维护难度，下一步最值得做的是：

- 把更多正文搬到 Markdown
- 模板层继续保留 Jekyll
- 队内同学以后主要只改 Markdown 和 YAML

这样会比继续手改 HTML 更稳。
