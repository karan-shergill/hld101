# Metric Monitoring Service

![metrics_monitoring](../practice/images/metrics_monitoring.png)

## Source

1. https://www.hellointerview.com/learn/system-design/problem-breakdowns/metrics-monitoring
2. https://youtu.be/FMsZbo1DJRg
3. https://dilipkumar.medium.com/metrics-monitoring-and-alerting-system-6e01b551aa89
4. https://blog.bytebytego.com/p/metric-monitoring

## Area to focus

1. How to scale WRITE of so many metrics date?
2. How to store metric related data in the database? How to scale READ of so many metrics date that will be query via dashboard?
3. How do we serve low-latency dashboard queries over weeks of data?
4. How do we reduce alert latency below 1 minute?
5. How do we ensure high availability during spikes and failures?

## New Learning

1. What is **Label, Metric and Series** that is needed for a metric monitoring?
2. Metric monitoring system is a kind of system where we need to hand both:
    1. Heavy WRITEs
    2. Heavy READs
3. Realtime alerts via Flink