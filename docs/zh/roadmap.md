# 路线图

[English](../roadmap.md) | [中文](./roadmap.md)

---

各阶段按“先窄后深”排序。每个阶段只有在验收标准通过后才算完成。

| 阶段 | 范围 | 验收标准 |
|---|---|---|
| **P0 —— 宪章** | 定位、技术设计、模板准备规范、DocMD 契约草案、模板清单语法与 schema、`SKILL.md` 和 `routing.md` 骨架、仓库规则 | 用户能照着规范整理模板；所有文档交叉链接都能解析 |
| **P1 —— 模板入库** | `template_inspect.py` 体检报告；Create Template 写出干净工作区（`reference.docx` + `template.yaml` + `README.md`）；docx-preview 块映射 spike | 在用户的一两个真实模板上，报告准确指出缺陷，工作区通过 schema 校验 |
| **P2 —— Quick 生成 + 实时预览** | `docmd_to_docx.py`（样式映射、槽位、带编号标题、列表、表头重复的表格、带题注的图片、脚注、目录域、交叉引用）；`docmd_checker.py`；Quick profile；每写完一节就刷新的只读实时预览 | 产物只使用模板样式，零直接格式；在 Word 中“更新域”后编号和域都正确；每写完一节预览即刷新 |
| **P3 —— Default 路线** | Planner 角色与两段确认（Stage 1 包含模板选择）；`doc_spec.md` / `doc_lock.md` / 账本；早期与终检闸门；引用与参考文献；预览批注与应用闭环；Word COM 精确预览 | 一万字以上的文档术语和语气保持一致；所有引用都能解析；批注 → 应用 → 重新导出全流程跑通 |
| **P4 —— 编辑已有 DOCX** | 已有 docx 的 roundtrip 工作区：填充、修订选定章节、修订模式、批注 | 未改动段落的 XML 逐字节不变 |
| **以后** | OMML 公式、原生图表、以内容控件作为结构化输入、多节附录 | 按能力准入测试逐项准入 |
