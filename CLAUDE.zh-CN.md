# CLAUDE.md(中文版)

本文件为 Claude Code(claude.ai/code)在此仓库中工作时提供指导。

> 说明:Claude Code 会自动读取的是英文版 `CLAUDE.md`;本文件是它的中文对照翻译,供人工阅读参考。如两者不一致,以 `CLAUDE.md` 为准。

## 这是什么

"Codelab" 是一组独立的、配有插图、可交互的 HTML 讲解页面,面向计算机/数学主题(目前包括模运算、素性检测,以及快速幂)。仓库中没有构建系统、包管理器或测试套件——每个页面都是一个自包含的 `.html` 文件,样式与脚本均内联在文件中,可直接在浏览器中打开,也可作为静态文件部署。

## 常用命令

本仓库没有构建、lint 或测试工具。预览页面只需直接打开:

```bash
open index.html                 # macOS
# 或本地起一个静态服务器,例如:
python3 -m http.server
```

所有验证都是手动的:在浏览器中打开文件,实际操作页面上的交互组件(输入框、"运行"按钮、`<details>` 测验)。

## 站点结构

- `index.html` / `index_cn.html` —— 首页(英文 / 中文),通过 `.card` 链接指向各讲解页面。
- `prime_detective_en.html` / `prime_detective_cn.html` —— 核心内容:一个由 8 个"案件"组成的叙事("素数侦探社"),依次讲解余数、模加法、快速幂、素性检测、埃拉托斯特尼筛法,以及费马 / Miller–Rabin 素性检测。各小节结构为 `<section class="case" id="caseN">`。
- `fast_exponentiation_en.html` / `fast_exponentiation_zh.html` —— 一个更深入的"技能补充"侧篇,展开讲解主案件文件中 Case 03 的内容,结构为 `<section class="level" id="levelN">`,共 1–8 步。

**注意语言后缀并不统一**:index 和 prime_detective 系列页面使用 `_cn` 后缀,而 fast_exponentiation 系列使用 `_zh` 后缀。编辑已有文件时请沿用其原本的后缀,不要重命名。

## 中英文配对约定

每个内容页面都以中英文一对的形式存在,二者结构必须保持一致:相同的 section ID、相同的交互组件 DOM 结构、相同的内联 JS 逻辑——只有文案和标签是翻译过的。修改其中一种语言的内容或行为时,务必在对应的另一语言版本中同步修改。每个页面的 `.lang-switch` 直接链接到其另一语言的对应页面(而不仅仅是返回首页)。

## 页面结构(所有内容页共用的约定)

每个讲解页都遵循相同的模板,理解了一个页面就能很容易上手编辑其他页面:

- **主题 / 配色**:CSS 自定义属性(`--paper`、`--ink`、`--accent`、`--code-bg` 等)统一在 `:root` 上定义一次,深色模式通过两种方式覆盖:一是 `@media (prefers-color-scheme: dark)`(目前唯一实际生效的方式),二是 `:root[data-theme="dark"]` / `:root[data-theme="light"]`(目前是尚未使用的预留钩子,现在没有任何 JS 代码去设置 `data-theme`)——如果修改配色,请同时保持这两处同步,以便未来添加手动切换主题功能时可以直接使用。
- **可复用的布局组件**(各页面之间共用同一套 class 名):`.hero`(标题横幅)、`.level` / `.case` 编号小节、`.callout` 提示框(`.fact` / `.tip` / `.warn` 三种样式)、`.codebox` 语法高亮代码块(用 `.kw` / `.cm` / `.st` / `.nu` 分别表示关键字/注释/字符串/数字)、`.annot` / `.annot-row` 代码配旁注的讲解形式、`.chain` / `.bits` 用于可视化倍增/二进制过程、`.stats` 数据统计卡片、`table.trace` 用于逐步展示算法执行轨迹、`.widget` 交互工具容器,以及 `<details>` 用于折叠式测验题(答案默认隐藏)。
- **交互组件**:统一实现在页面末尾的一个纯 `<script>` 块中(IIFE 写法,不依赖任何外部库)。组件读取 `<input>` 的值,用 JS 重新实现与 `.codebox` 中展示的 Python 代码等价的算法逻辑,并将逐步结果渲染进 `table.trace`,插入到 `#out` 容器中。新增交互演示时,请遵循这一模式,不要引入前端框架。
- 每个页面结尾都有一个 `.footer`,用于交叉引用系列中的其他案件/页面(例如 fast_exponentiation 页面的页脚指回主案件文件;主案件文件 Case 03 中则链接到 fast_exponentiation 作为可选的深入阅读)。

## 编辑注意事项

- 保持页面零外部依赖(不要引入外部 CDN 脚本/字体/样式表)——所有内容都内联在文件中,这样即使把文件当作本地原始文件打开也能正常工作。
- 新增讲解页面时,请同时创建中英文两个版本,并从 `index.html` / `index_cn.html` 中链接过去,同时在两个新页面之间加上 `.lang-switch` 互相跳转的链接。
