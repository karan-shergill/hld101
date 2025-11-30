# Google Docs

## Source

1. https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs
2. https://systemdesignschool.io/problems/google-doc/solution
3. https://vishal-vishal-gupta48.medium.com/google-docs-hld-high-level-system-design-d2e62f1d7ff5

## Area to focus

1. If multiple users are making edit to same text, how will we resolve the conflicts?
2. How will we see live cursor position of all the online users?
3. How do we scale to millions of web socket connections? How can Zookeeper/Console/Hash Ring can help us here? How consistence hashing help us here?
4. How do we keep storage under control? How we do Compaction and keep multiple snapshot / version?
5. Document URL creation on sharing (Mask the original document path)
6. Database design & query pattern.
7. How to keep permission data? (OWNER/EDITOR/COMMENTER/VIEWER)

## New Learning

1. Algorithms for realtime conflict resolution OT(Operational Transformation) & CRDT (Conflict-free Replicated Data Types)
2. If a service has multiple instance, how to handle them?