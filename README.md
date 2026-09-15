# Recycled-content annotation dataset (public release)

This folder is a public-release export of the annotated samples behind the
ICWSM "AI slop" / Recycled social media text paper. It contains two files:

- `recycled_clusters_sample.csv` — 9,970 recycled-text clusters
  each labelled by three LLMs.
- `threads_sample.csv` — 18,773 self-reply thread continuations, each
  concatenated into one document and labelled by the same three LLMs.

- Note: The numbers may not match by a small margin as 78 potentially unsafe posts were removed.

This is a public research-data release accompanying the paper.

## `recycled_clusters_sample.csv`

| Column | Meaning |
|---|---|
| `cluster_id` | Stable id (a hash of the cluster's normalized text). Not a tweet or user id. |
| `media_type` | `text` or `media`. |
| `media_subtypes` | `\|`-joined list of media types attached to the representative post (photo/video/animated_gif), if any. |
| `language` | ISO-ish language code assigned during collection. |
| `content` | The representative post's raw text. |
| `content_normalized` | Lowercased, URL/@mention-stripped, punctuation-stripped version (approximates, but is not byte-identical to, the internal DuckDB/RE2 canonicalization used to detect duplicates). |
| `length` | Character length of `content`. |
| `number_of_authors` | Distinct authors who posted this text (`n_authors` from the frozen sample draw). |
| `years_recycled` | JSON list of the distinct calendar years in which the cluster's posts occurred, e.g. `[2018, 2020, 2021]`. |
| `engagement_counts` | JSON list of per-post engagement (likes + retweets), one value per surviving post in the cluster, ordered by post date. |
| `qwen_family` / `qwen_subclass` | Qwen3.5-397B-A17B's label. |
| `gpt5mini_family` / `gpt5mini_subclass` | GPT-5 mini's label. |
| `sonnet_family` / `sonnet_subclass` | Claude Sonnet 5's label. |
| `plurality_family` / `plurality_subclass` | Majority vote across the three models; empty if all three disagreed (no majority). |

Taxonomy (5 families / 15 subclasses): `expression` (aphorism,
everyday_reflection, constructed_joke, creative_writing), `assertion`
(commentary_news, fact_trivia, instructional_listicle,
personal_announcement), `solicitation` (commercial_solicitation,
engagement_solicitation, contest_giveaway), `borrowed` (secular_quote,
scripture_quote, song_lyric), `non_content` (degenerate_filler).

## `threads_sample.csv`

One row per self-reply thread (a tweet plus its author's own chronological
reply chain).

| Column | Meaning |
|---|---|
| `content` | All of the thread's tweets concatenated in chronological order (newline-separated), i.e. the thread's full text. |
| `content_normalized` | Same normalization as the clusters file. |
| `length` | Character length of `content`. |
| `n_thread_tweets` | Number of tweets concatenated into `content`. |
| `qwen_family` / `qwen_subclass`, `gpt5mini_family` / `gpt5mini_subclass`, `sonnet_family` / `sonnet_subclass`, `plurality_family` / `plurality_subclass` | Same taxonomy and voting rule as the clusters file, applied to the full concatenated thread text. |


## What was removed for privacy

- **No author/user IDs, usernames, or handles, or tweet ids** are shared
  anywhere in either file.
