# Star Explorer Robotics Website

这是基于 Jekyll 重构后的机器人队网站，内容结构来自 `Robocup_TDP_starexplorer.pdf`。

## 快速说明

- 公网站点重点展示：队伍、实物图片、基础硬件配置、比赛准备情况
- 公开页面默认不再主动暴露具体算法和内部实现细节
- 日常维护优先改 `_data/*.yml`，不要先改页面模板

## 常用文件

- `_data/team.yml`：队伍信息、简介、成员
- `_data/hardware.yml`：硬件模块、参数表、设备清单
- `_data/competition.yml`：比赛准备、实验进展、路线图
- `_data/resources.yml`：资源链接
- `assets/images/`：网页图片
- `assets/css/main.css`：整站样式

## 详细维护文档

请优先看：

```text
WEBSITE_MAINTENANCE.md
```

里面已经写清楚：

- 改成员
- 改硬件表格
- 换图片
- 新增页面
- 本地预览
- GitHub Pages 发布

## 本地预览

```bash
bundle install
bundle exec jekyll serve
```

默认地址：

```text
http://127.0.0.1:4000
```
