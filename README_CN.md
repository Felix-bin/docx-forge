# docx-forge

[English](./README.md) | [中文](./README_CN.md)

在与 AI Agent 的对话中，把文档写**进你自己的模板**，产出原生样式化、可编辑的 Word 文档。

docx-forge 先理清文档的论证结构，再用受约束的 Markdown 方言（DocMD）逐节写作，最后编译进你的 Word 模板：每个段落都使用模板样式，标题编号、题注、交叉引用和目录全部由 Word 自身生成；写作过程中，浏览器实时预览会显示编译后的文档。

> **状态：P0 —— 宪章与骨架。** 设计文档和契约已就位；脚本、路线和预览尚未实现。见[路线图](./docs/zh/roadmap.md)。

## 为什么

AI 写出的 Word 文件往往看起来没问题，一编辑就坏：假标题、手打编号、手工目录、术语不一致。docx-forge 把你的模板当作模具，把 Word 自身的机制当作结构的唯一来源，所以你继续编辑时结果依然正常。

## 从这里开始

| 如果你想…… | 阅读 |
|---|---|
| 现在就开始整理 Word 模板 | [模板准备规范](./docs/zh/template-preparation-guide.md) |
| 了解项目是什么、不是什么 | [项目定位](./docs/zh/project-positioning.md) |
| 了解它如何工作 | [技术设计](./docs/zh/technical-design.md) |
| 查看各阶段交付内容 | [路线图](./docs/zh/roadmap.md) |

## 致谢

工作流架构参考了 Hugo He 的 [PPT Master](https://github.com/hugohe3/ppt-master)（MIT）。见 [NOTICE.md](./NOTICE.md)。

## 许可证

[MIT](./LICENSE)
