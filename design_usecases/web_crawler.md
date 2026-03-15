# Web Crawler

![Web Crawler System](../practice/images/web_crawler_system.png)

## Source

1. https://www.hellointerview.com/learn/system-design/problem-breakdowns/web-crawler
2. https://youtu.be/6u25GckPhLU
3. https://bytebytego.com/courses/system-design-interview/design-a-web-crawler

## Area of focus

1. How can we ensure we are fault tolerant and don't lose progress?
2. How can we ensure politeness and adhere to robots.txt?
3. How to scale to 10B pages and efficiently crawl them in under 5 days?
4. How to handle rate limiting?
5. How to handle URL prioritisation?

## New Learning

1. Web Crawler terms
    1. Seed URL
    2. URL Frontier
    3. DNS resolver
    4. HTML Downloader
    5. etc…
2. How async system works with queues
3. How to scale system like this that need a lot of data & memory