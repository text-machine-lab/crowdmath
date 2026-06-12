# CrowdMath Dataset

## Overview

This dataset captures the collaborative mathematical reasoning that took place
on the MIT PRIMES CrowdMath online research program. In CrowdMath, high-school
and undergraduate students work together on open research problems in
mathematics, posting conjectures, proofs, error corrections, and questions on a
shared message board (Art of Problem Solving / AoPS).

Expert annotators read every thread and labeled each post with its role in the
mathematical discourse. Those annotations were then grouped into **progress
chains** -- sequences of posts that together advance a single mathematical
result from an initial claim to a verified proof.

The dataset file `dataset.json` is a JSON array of progress chains.

## File format

`dataset.json` is a JSON array. Each element is a **progress chain** object
with the fields described below.

### Progress chain fields

| Field | Type | Description |
|---|---|---|
| `result_id` | string | Unique identifier for the chain, formatted as `<topic_id>-<post_number>` with an optional letter suffix (e.g. `"1228277-14"`, `"1320553-27a"`). |
| `project_id` | string | CrowdMath project identifier (e.g. `"mitprimes2016"`, `"mitprimes2021"`). Each project corresponds to one year of the program; some years have multiple projects (e.g. `"mitprimes2024"` and `"mitprimes2024-2"`). |
| `open_problem_ref` | string or null | The open problem reference as written by the annotator (e.g. `"2020-6"`, `"2017-5"`). Null when the chain could not be matched to a specific open problem. |
| `open_problem_year` | integer or null | Four-digit year of the matched open problem. Null when unmatched. |
| `open_problem_number` | integer or null | Problem number within that year's problem set. Null when unmatched. |
| `problem_text` | string or null | The mathematical problem statement that this chain addresses. When an open problem was matched, this includes official problem text from the CrowdMath problem set. Otherwise it contains the text of the first "Problem" post in the thread. |
| `problem_resources` | string or null | Supplementary resources (references, hints, relevant background) associated with the open problem. Non-null for only a small number of chains. |
| `posts` | array | Ordered list of post objects that form this chain (see below). The Problem post is excluded from this array because its content is captured in `problem_text`. Posts are sorted by `post_order`. |

### Post fields

Each element of the `posts` array is an object with these fields:

| Field | Type | Description |
|---|---|---|
| `topic_id` | string | AoPS forum thread ID. |
| `post_number` | integer | 1-based position of this post within its thread. |
| `post_order` | integer | Global ordering index across the entire dataset. Use this field to sort posts chronologically. |
| `post_id` | string | AoPS database identifier for this post. |
| `project_id` | string | CrowdMath project this post belongs to (same format as the chain-level `project_id`). |
| `title` | string | Title of the forum thread this post appears in. |
| `text` | string | Full text of the post in BBCode markup (see "Text format" below). |
| `thanked` | integer | Number of "thank" reactions this post received from other users. |
| `comment_count` | integer | Number of comments on the thread at the time the data was collected. |
| `labels` | array | Annotation labels assigned to this post (see below). A post may have multiple labels. |

### Label fields

Each element of the `labels` array is an object with these fields:

| Field | Type | Description |
|---|---|---|
| `label_type` | string | The annotation category (see "Label types" below). |
| `result_ref` | string | The progress chain this label contributes to, formatted as `<topic_id>-<post_number>`. For `Published-in-paper` labels, this is an arXiv ID and theorem reference instead (e.g. `"1704.05211,Theorem-2.3"`). |
| `prev_ref` | string or null | A back-reference to a specific earlier post, formatted as `<topic_id>-<post_number>`. Used by `Answer` (points to the `Question` it responds to) and `FindError` (points to the post whose error is identified). Null for all other label types. |

## Label types

Labels describe the role a post plays in the mathematical discourse. They fall
into two categories.

### Discourse labels

These labels indicate how a post contributes to the progress chain:

| Label | Meaning |
|---|---|
| `Start` | Introduces an initial claim, conjecture, or approach for a result. |
| `Progress` | Extends or partially advances an existing line of reasoning. |
| `NewProgress` | Introduces a new direction or substantially different approach to an existing result. |
| `Proof` | Provides a complete proof of the result. |
| `NewProof` | Provides a complete proof using a substantially different method than prior proofs. |
| `Question` | Asks a mathematical question relevant to the result. |
| `Answer` | Responds to a specific `Question` (see `prev_ref`). |
| `FindError` | Identifies a mathematical error in a specific earlier post (see `prev_ref`). |
| `Erroneous` | Marks a post whose mathematical content was found to contain an error. |
| `Result` | Marks the post that states the final, verified result of the chain. Often co-occurs with `Proof`. |

### Metadata labels

| Label | Meaning |
|---|---|
| `Published-in-paper` | Indicates this result was published in a peer-reviewed paper. The `result_ref` field contains the arXiv ID and theorem number. |

## Project IDs

Each `project_id` corresponds to one CrowdMath research project:

| Project ID | Year | Chains |
|---|---|---|
| `mitprimes2016` | 2016 | 26 |
| `mitprimes2017a` | 2017 | 59 |
| `mitprimes2018` | 2018 | 6 |
| `mitprimes2019` | 2019 | 11 |
| `mitprimes2020` | 2020 | 21 |
| `mitprimes2021` | 2021 | 19 |
| `mitprimes2022` | 2022 | 8 |
| `mitprimes2023` | 2023 | 2 |
| `mitprimes2024` | 2024 | 3 |
| `mitprimes2024-2` | 2024 | 2 |
| `mitprimes2025` | 2025 | 5 |
| `mitprimes2025-2` | 2025 | 2 |
