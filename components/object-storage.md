# Object Storage (Blob Store, e.g., S3)

> Questions to know before using object storage as a building block in a system design interview.

## Core Concepts

- What problem does object storage solve that a filesystem or database can't (near-unlimited scale, durability, cost for unstructured blobs)?
- What's the difference between object storage, block storage, and file storage, and when do you use each?
- What is an object key, and why is object storage a flat namespace rather than a real directory hierarchy?

## Durability & Consistency

- How does object storage achieve very high durability (e.g., "11 nines") — what's the role of erasure coding/replication across facilities?
- What consistency model does object storage provide (historically eventual, now often strong read-after-write), and what does that mean for a client that just wrote and immediately reads?

## Access Patterns

- What is multipart upload, and why is it needed for large files?
- What is a pre-signed URL, and how does it let a client upload/download directly without proxying through your backend?
- How do you serve object storage content at low latency globally (put a CDN in front of it)?
- What's the difference between hot, warm, and cold storage tiers, and how does that affect cost/latency trade-offs?

## Design Considerations

- Where do you store metadata about an object (owner, size, permissions) — in the object store itself or a separate database?
- What happens on a partial upload failure, and how do you avoid orphaned/incomplete objects?
- When is object storage NOT the right choice (small, frequently mutated records; strong transactional needs)?

## Interview Usage

- How would you design a "Dropbox/Google Drive"-style system using object storage plus a metadata database?
- How would you design photo/video storage for a social app (upload flow, thumbnail generation, CDN delivery)?
