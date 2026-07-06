# Knowledge Base for Probability on Trees and Networks

这个目录用于把《Probability on Trees and Networks》以及后续相关论文整理成一个面向 math agent 的可检索知识库。目标不是简单地把 PDF 切块，而是把书中的定义、定理、证明技巧、直觉、例子和依赖关系拆成可组合的知识单元。

## 设计目标

这个知识库同时服务两类需求：

- 按书本结构学习：保留章节、小节、页码和原始出处，方便回到文本。
- 按证明任务检索：把 theorem、definition、technique、intuition 等提升为一等节点，方便 agent 在解题或证明时快速找到可用 premise 和 proof strategy。

因此，章节不是唯一索引。章节更像导航骨架，真正被 agent 使用的是定理、技巧、直觉和它们之间的依赖边。

## 当前目录

```text
knowledge/
  README.md
  schema/
    minimal_node_schema.yaml
  book/
    ch02_random_walks_electrical_networks/
      electrical_networks_sample.yaml
  papers/
    2022_hutchcroft_anchored_isoperimetric_dimension/
      paper_graph.yaml
  retrieval_packs/
    ch02_recurrence_via_cutsets.md
```

其中：

- `schema/minimal_node_schema.yaml` 定义最小节点类型、边类型和检索策略。
- `book/.../electrical_networks_sample.yaml` 是第 2 章电网络部分的样例知识图谱。
- `papers/.../paper_graph.yaml` 是论文接入样例，把论文贡献映射到书本知识骨架上。
- `retrieval_packs/ch02_recurrence_via_cutsets.md` 是面向 agent 的压缩检索包，展示如何把多个节点组合成一个证明 recipe。

## 节点类型

最小 schema 目前包含这些节点：

- `ChapterSection`：章节或小节节点，用于导航、学习路径和来源定位。
- `Definition`：定义、符号、别名、直觉和相关概念。
- `Theorem`：定理或命题，包括假设、结论、证明大纲和使用的技巧。
- `Technique`：可迁移的证明技巧，例如 cutset bound、second moment、mass transport。
- `Intuition`：非形式化解释，帮助 agent 选择正确的 proof direction。
- `Exercise`：书中练习，尤其是会被后文使用的 in-text exercises。
- `Paper`：论文级元数据，包括主贡献、book anchors 和抽取出的节点。
- `Corollary`：论文中的推论，常用于连接新定理和已有书本知识。
- `OpenQuestion`：论文提出或强调的开放问题。

后续可以扩展 `Example`、`Counterexample`、`OpenQuestion`、`LiteratureLink`、`FormalizationCandidate` 等节点。

## 边类型

图谱中的边用于表达知识如何被使用，而不仅是文本相邻关系。常用边包括：

- `depends_on`：某个定理依赖某个定义、lemma 或 exercise。
- `uses_technique`：某个证明使用某种技巧。
- `has_intuition`：定理或定义对应的直觉解释。
- `generalizes` / `special_case_of`：表达推广关系，后续接论文时很重要。
- `equivalent_to`：不同表述之间的等价，例如随机游走与电网络表述。
- `analogy_with`：结构类比，例如 capacity、branching number、Hausdorff dimension。
- `formalizes_as`：未来连接 Lean/Mathlib 或其他 proof assistant 时使用。
- `connects_to`：论文或节点连接到书本中的主题节点。
- `introduces`：论文引入新的 technique、definition 或 viewpoint。
- `refines`：论文强化、细化或给出 perturbative 版本的已有结论。
- `answers_question`：论文回答某个 conjecture 或 open question。
- `arises_from`：开放问题从某个定理、技巧或现象中产生。

## 编写一个新小节

建议每个小节至少抽取：

1. 一个 `ChapterSection` 节点。
2. 关键 `Definition` 节点。
3. 关键 `Theorem` / `Lemma` 节点。
4. 至少一个 `Technique` 或 `Intuition` 节点。
5. 必要的 `depends_on`、`uses_technique`、`has_intuition` 边。

对于 agent 来说，`proof_plan` 和 `used_techniques` 很关键。不要只写定理全文，也要写“证明为什么会这么走”。

## Retrieval Pack

`retrieval_packs/` 里的文件不是原始知识图谱，而是给 agent 使用的压缩上下文。它适合回答某一类任务，例如：

- 如何证明 recurrence？
- 如何构造 finite-energy flow 证明 transience？
- 如何使用 mass-transport principle？
- 如何用 second moment method 证明 percolation survival？

一个 retrieval pack 应该包含：

- 适用 query。
- 核心 dictionary。
- main trick。
- 关键 premises。
- agent use pattern。
- common pitfalls。

这类文件可以由图谱节点自动生成，也可以先手写作为 gold examples。

## 接入一篇论文

论文入库时不要只存 PDF chunk。推荐先创建一个 `paper_graph.yaml`，把论文看作对书本知识骨架的增量。

每篇论文至少包含：

- 一个 `Paper` 节点，记录标题、作者、年份、来源、摘要总结、主贡献和 book anchors。
- 若干 `Theorem` / `Corollary` 节点，记录核心数学结论。
- 若干 `Technique` 节点，记录可迁移的 proof trick。
- 必要时加入 `Definition`、`OpenQuestion`、`Counterexample` 等节点。
- 用 `connects_to`、`uses_technique`、`generalizes`、`refines`、`answers_question` 等边挂回书本节点。

当前样例是 Hutchcroft 2022：

```text
papers/2022_hutchcroft_anchored_isoperimetric_dimension/paper_graph.yaml
```

这篇论文连接到书中的 transience/effective conductance、isoperimetric inequalities、percolation on transitive graphs 等主题。它的主要增量是把 supercritical percolation clusters 的 anchored isoperimetric dimension 与 finite-cluster tail、boundary-to-infinity connection estimates 等价起来，并推出 transient transitive graph 在 `p` 充分接近 1 时无限簇 transient。

## 后续扩展路线

短期可以按章节推进：先抽第 2 章电网络，因为它连接随机游走、UST/USF、recurrence/transience 和后续很多技巧。之后可以继续抽第 5 章 second moment/percolation、第 8 章 mass transport、第 10 章 uniform spanning forests。

接入论文时，不建议把论文只作为 PDF chunk 入库。更好的做法是把论文贡献映射到已有节点：

- 它推广了哪个 theorem？
- 它引入了哪个 technique？
- 它解决或改写了哪个 open question？
- 它给出了哪个 counterexample？
- 它是否有可 formalize 的 statement？

最终目标是形成一个同时支持自然语言解释、premise selection、proof strategy retrieval 和 formalization linking 的数学知识库。
