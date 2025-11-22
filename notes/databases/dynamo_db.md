# Dynamo DB

## Architecture

1. Read: [**A Deep Dive into Amazon DynamoDB Architecture**](https://blog.bytebytego.com/p/a-deep-dive-into-amazon-dynamodb)
2. Read: [**DynamoDB Architecture**](https://medium.com/@joudwawad/dynamodb-architecture-5a38761501a7)
3. See: [**DynamoDB Deep Dive**](https://www.youtube.com/watch?v=2X2SO3Y-af8)
4. See: [**Amazon DynamoDB - Paper Explained**](https://youtu.be/LnqKfLcszEg)
5. See: [**Amazon DynamoDB: A Scalable, Predictably Performant, and Fully Managed NoSQL Database Service**](https://youtu.be/cU01EnyBwQI)

## Keys

1. Read: [**Core components of Amazon DynamoDB**](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)
2. Read: [**DynamoDB Keys**](https://dynobase.dev/dynamodb-keys/)
3. See: [**AWS DynamoDB Schema Design**](https://youtu.be/XvD2FrS5yYM)
4. See: [**What is a DynamoDB GSI (Global Secondary Index)**](https://youtu.be/ihMOlb8EZKE)
5. See: [**What is a DynamoDB LSI (Local Secondary Index)**](https://youtu.be/Y8gMoZOMYyg)
6. LSI vs GSI

| Feature | LSI | GSI |
| --- | --- | --- |
| Partition Key | **Same as base table** | **Can be different** |
| Sort Key | Different from base table | Optional |
| Create After Table Creation? | ❌ No | ✅ Yes |
| Read Consistency | Strong + Eventual | Eventual only |
| Scaling | Shares throughput with table | Scales independently |
| Use Case | Need different sorting for same entity | Need different access patterns |