# Financial Modelling Skills — LBO Model

用于 Codex 的杠杆收购（LBO）建模 skill，支持创建、填充、修复和审阅公式驱动的 Excel 模型。

A self-contained Codex skill for building and auditing formula-driven leveraged buyout models.

## 能做什么 / Capabilities

- Sources & Uses、收购日备考资产负债表及三表联动。
- 非完整年度（stub period）、平均余额营运资本及利息计算。
- 分层债务、强制摊还、循环贷款、现金清偿和 HoldCo PIK。
- Sponsor IRR / MOIC、管理层 rollover、价值创造分解及敏感性分析。
- 静态工作簿结构、公式及已保存错误值检查。

## 安装 / Installation

仓库地址：https://github.com/ayeshah2820-maker/Financial-Modelling

在 Codex 中使用内置的 `$skill-installer`，提供本仓库地址及路径 `skills/lbo-model`，要求安装此 skill。

也可以下载本仓库，将 `skills/lbo-model` 文件夹复制到自己的 `$CODEX_HOME/skills/`（默认 `~/.codex/skills/`）。如果同名 skill 已存在，请先备份并比较差异。

Use Codex's built-in `$skill-installer` with this repository URL and the path `skills/lbo-model`, or copy that folder into `$CODEX_HOME/skills/` (default: `~/.codex/skills/`). Back up an existing installation before replacing it.

## 使用 / Usage

安装后，在 Codex 下一轮对话中调用：

```text
$lbo-model 根据我提供的历史财务报表和交易假设，搭建五年期 LBO Excel 模型。
包含 Sources & Uses、三表、债务现金瀑布、Sponsor IRR / MOIC、敏感性分析和 Checks。
```

```text
Use $lbo-model to audit the attached LBO workbook. Trace cash flow, debt paydown,
and sponsor returns; identify formula and economic inconsistencies.
```

请提供历史报表、交易日期、估值、融资条款和退出假设。此仓库包含工作流与审计工具，不包含公司数据或 Excel 模板。

## 审计脚本 / Audit helper

Python 3.9+，依赖 `openpyxl`：

```shell
python -m pip install -r requirements.txt
python skills/lbo-model/scripts/audit_lbo_model.py path/to/model.xlsx
```

输出为 JSON，含 errors、warnings、metrics 和 PASS / FAIL；出现 errors 时退出码为 1。脚本不会修改工作簿。

该脚本不会重新计算 Excel 公式。PASS 只代表脚本未发现其检查范围内的错误，不证明模型经济逻辑正确。文本引用、外部链接检测为启发式，可能产生误报；无公式缓存时也无法据此验证计算结果。应结合 native Excel 重算、经济逻辑核对和视觉检查使用。含循环引用的模型需确认迭代收敛。

The helper does not recalculate formulas. Its PASS result is limited to its static checks and available cached values. Review heuristic warnings manually and complete Excel recalculation, economic reconciliation, and visual inspection.

## 文件结构 / Contents

```text
skills/lbo-model/
  SKILL.md                       # Main instructions
  agents/openai.yaml             # Codex display and invocation metadata
  references/lbo-methodology.md  # Detailed modeling methodology
  scripts/audit_lbo_model.py     # Read-only static workbook audit
requirements.txt
```

共享版已去除对特定私人模型文件的依赖，保留通用建模规则。原本地安装不受影响。
