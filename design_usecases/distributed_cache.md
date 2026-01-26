# Distributed Cache

## Source
1. https://www.hellointerview.com/learn/system-design/problem-breakdowns/distributed-cache
2. https://medium.com/system-design-concepts/distributed-cache-system-design-9560f7dd07f2
3. https://ravisystemdesign.substack.com/p/interview-prep-designing-a-distributed
4. https://youtu.be/iuqZvajTOyA
5. https://youtu.be/DUbEgNw-F9c

## Area to focus
1. How do we ensure our cache is highly available and fault tolerant?
2. How do we ensure our cache is scalable?
3. How can we ensure an even distribution of keys across our nodes?
4. What happens if you have a hot key that is being read from a lot?
5. What happens if you have a hot key that is being written to a lot?
6. How do we ensure our cache is performant?

## New Learning
1. LRU implementation using HashMap & DLL
2. Hot key READ & WRITE - analysis and fix
3. Janitor - something like this can be used in many systems for cleaning
4. Zookeeper
5. Lightweight client library - what it is, how it helps
