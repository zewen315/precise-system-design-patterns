# Elasticsearch (Search & Indexing)

> Questions to know before using a search engine as a building block in a system design interview.

## Core Concepts

- What problem does a search engine solve that a database `LIKE`/full-text index can't at scale?
- What is an inverted index, and why is it the core data structure behind full-text search?
- What's the difference between a document, an index, and a shard in Elasticsearch?
- How does Elasticsearch tokenize and analyze text (analyzers, tokenizers, stemming), and why does that matter for search quality?
- What is relevance scoring (TF-IDF / BM25), and how does it rank results?

## Architecture & Scaling

- How does sharding work in Elasticsearch, and why is the number of primary shards hard to change after index creation?
- What's the difference between a primary and a replica shard, and how does that provide both availability and read scaling?
- How do you scale Elasticsearch differently for write-heavy vs read-heavy workloads?

## Consistency & Sync

- Why is Elasticsearch near-real-time rather than immediately consistent (refresh interval)?
- How do you keep Elasticsearch in sync with the system of record (dual writes vs CDC/streaming from the database)?
- What's the risk of dual writes to both a database and Elasticsearch, and how do you avoid inconsistency (e.g., outbox pattern, CDC via Kafka)?

## Failure Modes & Interview Usage

- What causes cluster issues like split-brain or an unassigned shard, and how do you detect/recover?
- When would you use Elasticsearch vs a database's built-in full-text search vs a dedicated vector database?
- How would you design autocomplete/typeahead or fuzzy search using Elasticsearch?
- How would you design search for a design like Twitter/Instagram post search, and what would you index vs leave in the primary DB?
