# 个人博客主页

一个**单文件**的个人博客站点：`index.html` 内联全部样式与脚本，素材放在 `images/`、`audio/`、`videos/`，双击即可打开 —— 不需要服务器、不需要构建、不依赖任何第三方库或 CDN。

视觉基调是浅色的「液态玻璃」：半透明玻璃卡片叠在樱花背景上，顶部是一条胶囊导航。全站只做浅色与深色两套主题。

**在线访问**：<https://wanfengteng.github.io/my-homepage/>

- 主仓库（Gitee）：<https://gitee.com/wan-feng-a/blog>
- 镜像仓库（GitHub）：<https://github.com/wanfengteng/my-homepage>

---

## 页面结构

站点是单页应用，通过顶部导航在十个视图之间切换，页面本身不刷新：

| 视图 | 内容 |
| --- | --- |
| `#home` 首页 | 个人名片、搜索、走马灯、三张动态卡、音乐卡 |
| `#about` 关于 | 简介四段、十一个技能标签、五个时间节点 |
| `#works` 作品 | 六张等尺寸作品卡，含三个 Gitee 源码入口 |
| `#talk` 说说 | 发布框、时间轴、点赞与评论 |
| `#photos` 照片 | 八张拍立得叠层，点击原地展开成网格 |
| `#videos` 视频 | 三个分类合集卡，点击后在下方展开视频网格 |
| `#essay` 杂谈 | 纯文字卡片列表 |
| `#posts` 文章 | 六张封面卡，点进去是内嵌阅读页 |
| `#archive` 归档 | 一条正弦曲线串起来的时光河流 |
| `#contact` 留言 | 联系方式、留言表单、二维码 |

页面按「固定层 / 视图层 / 浮层层 / 页脚」四层组织：固定层用 `position: fixed` 挂住背景、花瓣与导航；视图层是十个 `section.view`，只有带 `.active` 的那个显示；浮层层放登录浮窗、播放器、灯箱、阅读页。

## 技术栈

- **HTML5**：`nav` / `main` / `section` / `article` / `figure` / `figcaption` / `aside` / `footer` 语义化标签
- **CSS3**：Flex、Grid、圆角与连续曲率（`corner-shape: squircle`）、阴影、渐变、过渡、关键帧动画、`transform`、`backdrop-filter`、`clamp()`
- **JavaScript**：原生，无框架，无打包工具
- **Canvas 2D**：樱花花瓣逐帧下落
- **WebGL**：卡片边缘的液态折射着色器
- **localStorage**：说说、评论、点赞、主题偏好、登录状态

## 目录结构

```
.
├── index.html          # 全部结构 / 样式 / 脚本（单文件）
├── images/             # 头像、背景、作品封面、相册、文章封面、二维码
├── audio/              # 音乐卡曲目
└── videos/             # 视频卡片素材
```

## 本地运行

直接双击 `index.html`，或任意静态服务器：

```bash
python -m http.server 8000
```

然后打开 <http://localhost:8000> 。

## 几个实现要点

- **液态玻璃导航**：不用 WebGL，而是「配方玻璃」——低强度 `backdrop-filter` 模糊 + 白色渐变叠层 + 内描边亮边 + SVG `feDisplacementMap` 做的边缘位移贴图，位移量只在边缘最强，中央为零，配合极轻的色散（红/蓝通道 ±12%）做出折射感。
- **时光河流**：归档页的河线由三重正弦叠加生成（`0.5·sin + 0.3·sin + 0.2·sin`），卡片按奇偶上下交错挂在河面上，虚线连到河心；支持拖拽、滚轮与惯性滑动。
- **入场动画**：`.reveal` 块用 Web Animations API 逐块上浮，结束即 `cancel()` 释放 transform —— 否则 `fill: both` 的终态会永久压住后续的 `:hover` 效果。
- **视频页**：首屏只出现三个分类合集卡，点击后在下方展开该分类的视频网格；卡片按索引写入独立的 `translate` 偏移做错落，悬停只在原格内放大，避免布局重排导致指针追不上。
- **暗色模式**：`html.dark` + CSS 变量，切换时用一次圆形擦除过渡；偏好写入 `localStorage`，刷新保持。

## 作者

谭博文 · 山东女子学院 · 数字媒体技术 · 数媒 2603

课程：Web 前端开发 · 2026 年 11 月

> 站点内容与素材仅用于课程作业展示。