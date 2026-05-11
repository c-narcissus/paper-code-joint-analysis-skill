# 契约驱动的可复用 Skill 设计方法学

## 1. 方法学名称

推荐命名：

**契约驱动的可复用分析流水线**

英文：

**Contract-Driven Reusable Analysis Pipeline**

这个名字比“适配器”更准确。它不是只把一个接口转成另一个接口，而是先定义稳定产物契约，再让不同分析模块把结果写入同一组人类可读和机器可读产物，最后由一个通用静态 reader 展示。

## 2. 软件开发中的类比

它可以类比为几种模式的组合：

| 设计思想 | 在 skill 中的对应关系 |
| --- | --- |
| Contract-first / 契约优先 | 先定义 `analysis_bundle.json`、固定 Markdown 文件名、schema version，再写流程 |
| Artifact-View Separation / 产物-视图分离 | Markdown 和 JSON 是真实产物；网页只是通用 reader |
| Template Method / 模板方法 | skill 固定工作流，不同论文或项目只填不同内容 |
| Ports and Adapters / 端口与适配器 | 不同论文、代码仓库、领域机制适配到统一输出契约 |
| CI Quality Gate / 质量门禁 | 用脚本检查 schema、固定文件、公式、乱码、网页资源和完整性 |

如果只选一个主概念，建议使用：

**契约驱动的产物-视图分离架构**

如果强调执行流程，建议使用：

**契约驱动的可复用分析流水线**

## 3. 核心设计原则

1. **先定义产物契约，再实现分析流程。**
   不要先手写网页，也不要让每次分析生成随意结构的报告。先固定机器可读 JSON、固定 Markdown 文件名、schema version 和必填字段。

2. **网页是模板，不是结果本身。**
   静态网页只读取固定产物。换论文、换代码仓库时，不应该重写 `index.html`、`app.js` 或 `styles.css`。如果展示能力不足，应升级通用 reader 模板。

3. **人类报告和机器数据同时存在。**
   Markdown 是完整学习记录，适合人阅读；JSON 是结构化数据，适合网页加载、校验和二次处理。不能只生成网页，也不能把长篇解释只塞进 JSON。

4. **skill 模块化，方便替换升级。**
   把深度阅读、疑问求证、代码映射、实验分析、图表生成、网页构建、验证检查拆成独立模块。以后升级精读能力，只换精读模块；升级网页，只改 reader 模板；升级校验，只改 scripts。

5. **每个完整输出都必须能被检查。**
   至少提供 schema 校验、固定文件存在性检查、乱码检查、公式渲染检查、网页资源检查和图表检查。检查失败时，应修复后重新验证。

6. **复杂内容使用稳定表达。**
   公式使用可渲染 math block 或结构化 formula 字段；UML 和流程图使用 Mermaid 或固定图表格式；网页负责渲染，正常情况下不暴露 raw LaTeX 或原始图表源码。

7. **为复用而设计，不为单次展示而设计。**
   新功能优先变成 module、schema 字段、validator 或 reader 通用组件，而不是写在某个项目的临时页面里。

## 4. 可复用设计方法学提示词

```text
请为这个任务设计一个可复用的 Codex skill，采用“契约驱动的可复用分析流水线”思想。

核心要求：

1. 先定义稳定输出契约，而不是先写页面或临时报告。
   - 固定机器可读文件，例如 analysis_bundle.json。
   - 固定人类可读 Markdown 文件，例如 main_report.md、questions.md、mapping.md、validation_report.md。
   - 固定 schema/version，后续升级必须显式迁移。

2. 页面必须是通用静态 reader。
   - 网页只读取固定契约文件。
   - 不允许每换一个项目就重写 index.html、app.js、styles.css。
   - 如果展示能力不足，修改 reader 模板本身，而不是为单个案例写特例代码。

3. skill 必须模块化。
   - 把深度阅读、代码映射、实验分析、疑问求证、图表生成、网页构建、验证检查拆成独立 module。
   - 每个 module 只负责一种产物或一种判断。
   - 以后升级某个能力时，只替换对应 module，不重写整个 skill。

4. 人类报告和机器数据必须同时存在。
   - Markdown 是给人看的完整学习记录。
   - JSON 是给网页和校验脚本读取的结构化数据。
   - 不允许只生成网页，也不允许把长篇解释只塞进 JSON。

5. 输出必须可验证。
   - 提供 schema 校验脚本。
   - 提供固定文件存在性检查。
   - 提供乱码检查、公式渲染检查、网页资源检查、图表检查。
   - 如果检查失败，必须修正后重新验证，不能只说明“理论上可以”。

6. 公式、图表和复杂结构必须有稳定表达。
   - 公式用可渲染 math block 或结构化 formula 字段。
   - UML/流程图用 Mermaid 或固定图表格式。
   - 网页负责渲染，不展示 raw 源码，除非渲染失败作为 fallback。

7. 设计目标是复用，而不是一次性完成。
   - skill 的价值在于下一篇论文、下一个项目也能复用同一套模板和契约。
   - 任何新增能力都应优先变成模块、schema 字段或 reader 通用组件。
   - 避免把项目特定逻辑写死进页面代码或主流程。

请最终输出：
- skill 的目录结构；
- SKILL.md 主流程；
- modules/ 拆分方案；
- references/ 中的输出契约和 schema；
- scripts/ 中的验证和构建脚本；
- assets/ 中的静态 reader 模板；
- 一个最小示例，证明换输入项目时不需要重写网页代码。
```

## 5. 使用示例

### 示例 A：创建论文与代码联合分析 skill

```text
请根据“契约驱动的可复用分析流水线”思想，设计一个用于论文和开源代码联合分析的 Codex skill。

它必须生成：
- 完整论文解读 Markdown；
- 论文疑问与代码求证 Markdown；
- 理论到代码映射 Markdown；
- 实验到代码映射 Markdown；
- 机器可读 analysis_bundle.json；
- 通用静态网页 reader；
- schema 校验和网页检查脚本。

网页必须只读取固定产物，不允许针对每篇论文重写页面代码。
```

### 示例 B：创建审稿分析 skill

```text
请用“契约驱动的可复用分析流水线”设计一个论文审稿 skill。

固定输出：
- review_report.md；
- claim_evidence_audit.md；
- weakness_matrix.md；
- rebuttal_suggestions.md；
- review_bundle.json；
- site/index.html。

网页 reader 必须复用同一模板。不同论文只更新 Markdown 和 JSON，不改网页代码。
校验脚本必须检查评分项、主要 claim、证据强弱、缺失实验和最终审稿意见是否齐全。
```

### 示例 C：创建实验复现分析 skill

```text
请用“契约驱动的可复用分析流水线”设计一个实验复现 skill。

固定输出：
- environment_report.md；
- command_matrix.md；
- result_parser.md；
- reproduction_gaps.md；
- reproduce_bundle.json；
- site/index.html。

要求把仓库 README、环境文件、脚本入口、日志格式和结果表格映射到统一契约。
网页只展示这些固定产物，不能为每个项目重写。
```

## 6. 最小推荐目录结构

```text
my-reusable-skill/
  SKILL.md
  modules/
    intake.md
    analysis-core.md
    question-led-reading.md
    diagram-generation.md
    reader-generation.md
    validation.md
  references/
    output-contract.md
    bundle.schema.json
    reader-data-contract.md
  scripts/
    validate_bundle.py
    check_outputs.py
    build_static_reader.py
    check_reader.py
  assets/
    reader-template/
      index.html
      assets/
        app.js
        styles.css
      vendor/
        katex/
        mermaid.min.js
```

## 7. 一句话总结

**先规定“产物长什么样”，再让所有分析模块往这个契约里填内容，最后用一个固定 reader 展示，并用检查脚本守住质量。**
