# Rate Limiter

![rate_limiter](../practice/images/rate_limiter.png)

## Source
1. https://youtu.be/MIJFyUPG4Z4
2. https://youtu.be/FU4WlwfS3G0
3. https://youtu.be/CVItTb_jdkE
4. https://www.hellointerview.com/learn/system-design/problem-breakdowns/distributed-rate-limiter
5. https://bytebytego.com/courses/system-design-interview/design-a-rate-limiter
6. https://medium.com/geekculture/system-design-design-a-rate-limiter-81d200c9d392
7. https://systemdesignschool.io/problems/rate-limiter/solution

## Area of focus
1. Where would be place the rate limiting logic and why so?
2. How would we differentiate between different requesters?
3. How do we handle dynamic rule configuration?
4. How do we handle race condition to check and update redis?
5. How do we scale to handle 1M requests/second?
6. How do we ensure high availability and fault tolerance?
7. How do we handle hot keys (viral content scenarios)?

## New Learning
1. Rate Limiting Algos:
    1. Fixed Window
    2. Sliding Window
    3. Leaky Bucket
    4. Token Bucket
2. Lua script in Redis - Atomicity in redis to prevent race condition