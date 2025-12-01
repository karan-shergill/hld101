# Mouse Pointer Tracking

## Source

1. https://engineeringatscale.substack.com/p/designing-real-time-collaborative
2. https://www.canva.dev/blog/engineering/realtime-mouse-pointers/

## Area of focus

1. How will users see each other's mouse pointers in real time?
2. What happens when a new user joins a document?
3. How do you handle user disconnect / reconnect?
4. What data do you send per cursor update?
5. Where do you store the current cursor positions?

## New Learning

1. Two approaches in case collaboration is needed between multiple nodes/server of same service
    1. Redis Pub/Sub Based (Distributed Servers, No Fixed Owner)
    2. Single Authoritative Server per Document
   3. When to use which approach
2. Kafka vs Redis pub-sub
3. Consul - Helps to implement consistent hashing

