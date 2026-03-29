# Auction Service

![auction_service](../practice/images/auction_service.png)

## Source

1. https://www.hellointerview.com/learn/system-design/problem-breakdowns/online-auction
2. https://pyemma.github.io/How-to-design-auction-system/

## Area to focus

1. How can we ensure strong consistency for bids?
2. Realtime action updates sent to users taking part in an auction?
3. How can we ensure that the system displays the current highest bid in real-time?
4. Multiple user bidding at same time, how to do concurrency control for bids?

## New Learning

1. Race Condition - Redis Lua script
2. Hot Auction Problem
3. Event Sourcing