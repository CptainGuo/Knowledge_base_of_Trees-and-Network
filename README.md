# Knowledge Base of Trees and Networks

This repository is an experimental knowledge base for modern discrete probability, built around *Probability on Trees and Networks* by Russell Lyons and Yuval Peres and extended with related research papers.

The goal is not to store PDFs as raw chunks. Instead, the repository organizes mathematical knowledge into structured objects that a math agent can retrieve and use: definitions, theorems, proof plans, reusable techniques, intuitions, exercises, papers, and open questions.

## Motivation

Modern math agents and theorem-proving systems need more than textbook summaries. They need:

- precise premises for proof search,
- reusable proof strategies and tricks,
- informal intuition for choosing a promising direction,
- dependency links between definitions, lemmas, theorems, and papers,
- stable source references back to books and papers.

This repository is designed as a bridge between a human-readable mathematical library and a retrieval system for math agents.

## Current Sources

Primary book:

- `book_pb.pdf`: *Probability on Trees and Networks*, Russell Lyons and Yuval Peres.

Example paper:

- `2207.05226v1.pdf`: Tom Hutchcroft, *Transience and anchored isoperimetric dimension of supercritical percolation clusters*.

## Repository Structure

```text
.
├── README.md
├── book_pb.pdf
├── 2207.05226v1.pdf
└── knowledge/
    ├── README.md
    ├── schema/
    │   └── minimal_node_schema.yaml
    ├── book/
    │   └── ch02_random_walks_electrical_networks/
    │       └── electrical_networks_sample.yaml
    ├── papers/
    │   └── 2022_hutchcroft_anchored_isoperimetric_dimension/
    │       └── paper_graph.yaml
    └── retrieval_packs/
        └── ch02_recurrence_via_cutsets.md
```

## Knowledge Model

The core schema lives in:

```text
knowledge/schema/minimal_node_schema.yaml
```

The schema treats mathematical content as a graph. Current node types include:

- `ChapterSection`: navigation nodes for book structure.
- `Definition`: formal definitions, aliases, and intuition.
- `Theorem`: statements, assumptions, conclusions, proof plans, and techniques used.
- `Technique`: reusable proof methods such as cutset bounds or cluster repulsion.
- `Intuition`: informal explanations that help guide search.
- `Exercise`: exercises that serve as dependencies or training tasks.
- `Paper`: paper-level metadata and contribution summaries.
- `Corollary`: derived results, often connecting paper contributions back to the book.
- `OpenQuestion`: research-frontier questions.

Edges describe mathematical relationships such as:

- `depends_on`
- `uses_technique`
- `has_intuition`
- `generalizes`
- `special_case_of`
- `equivalent_to`
- `connects_to`
- `refines`
- `answers_question`

## Book Integration

Book content is stored under:

```text
knowledge/book/
```

The current example is:

```text
knowledge/book/ch02_random_walks_electrical_networks/electrical_networks_sample.yaml
```

This sample extracts part of Chapter 2 into structured knowledge nodes around:

- effective conductance,
- energy of flows,
- Thomson's principle,
- Rayleigh monotonicity,
- Nash-Williams inequality,
- recurrence and transience.

The important design choice is that a chapter is only the navigation layer. The agent-facing units are the definitions, theorems, techniques, intuitions, and graph edges extracted from the chapter.

## Paper Integration

Papers are stored under:

```text
knowledge/papers/
```

The current example is:

```text
knowledge/papers/2022_hutchcroft_anchored_isoperimetric_dimension/paper_graph.yaml
```

A paper should be connected back to the book graph rather than stored as isolated notes. For each paper, the recommended extraction is:

- a `Paper` node with metadata, source, abstract summary, main contributions, and book anchors;
- key `Theorem` and `Corollary` nodes;
- reusable `Technique` nodes;
- relevant `Definition` and `OpenQuestion` nodes;
- graph edges such as `connects_to`, `uses_technique`, `refines`, `generalizes`, or `answers_question`.

The Hutchcroft 2022 example connects supercritical percolation clusters to the book's themes of transience, isoperimetry, and percolation on transitive graphs.

## Retrieval Packs

Retrieval packs live in:

```text
knowledge/retrieval_packs/
```

These are compact, task-oriented contexts for a math agent. They are not the primary source of truth. They are closer to compiled proof cards generated from the structured graph.

For example:

```text
knowledge/retrieval_packs/ch02_recurrence_via_cutsets.md
```

This pack gives an agent a ready-to-use recipe for proving recurrence via cutsets and the Nash-Williams inequality.

Long term, retrieval packs should mostly be generated from graph nodes. A few manually written packs can serve as gold examples for style and evaluation.

## Recommended Workflow

For a new book section:

1. Create a `ChapterSection` node.
2. Extract key `Definition` nodes.
3. Extract key `Theorem` or `Corollary` nodes.
4. Add `proof_plan`, `used_techniques`, and `intuition`.
5. Add graph edges such as `depends_on`, `uses_technique`, and `has_intuition`.

For a new paper:

1. Create a paper directory under `knowledge/papers/`.
2. Add a `paper_graph.yaml`.
3. Create a `Paper` node.
4. Extract the main theorem-level contributions.
5. Identify the new proof techniques.
6. Link the paper back to existing book nodes with `connects_to`, `refines`, `generalizes`, or `answers_question`.

## Near-Term Roadmap

Useful next targets:

- Expand Chapter 2 around random walks, electrical networks, recurrence, and transience.
- Add Chapter 5 nodes for second-moment methods, branching processes, and percolation.
- Add Chapter 6 nodes for isoperimetric inequalities and anchored expansion.
- Add Chapter 8 nodes for mass transport and invariant percolation.
- Add Chapter 10 nodes for uniform spanning forests.
- Build scripts to validate schema consistency and eventually generate retrieval packs automatically.

## Status

This repository is currently in the schema and example-building stage. The existing files are intended as templates for future extraction, not as a complete knowledge base yet.
