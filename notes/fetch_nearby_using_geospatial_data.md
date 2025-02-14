# Fetch Nearby Using Geospatial Data

- [Some Basics](#some-basics)
    - [What is Geospatial Data?](#what-is-geospatial-data)
    - [What is Geospatial Indexing?](#what-is-geospatial-indexing)
    - [What is Spatial Partitioning?](#what-is-spatial-partitioning)
- [Two-Dimensional Search (Brute Force)](#two-dimensional-search-brute-force)
    - [How It Works:](#how-it-works)
    - [Pros:](#pros)
    - [Cons:](#cons)
    - [Use Cases:](#use-cases)
- [Evenly Divided Grid](#evenly-divided-grid)
    - [How It Works:](#how-it-works)
    - [Pros:](#pros)
    - [Cons:](#cons)
    - [Use Cases:](#use-cases)
- [Geohash](#geohash)
    - [How It Works:](#how-it-works)
    - [Encoding Process:](#encoding-process)
    - [Example:](#example)
    - [Pros:](#pros)
    - [Cons:](#cons)
    - [Use Cases:](#use-cases)
- [Quadtree](#quadtree)
    - [How It Works:](#how-it-works)
    - [Structure:](#structure)
    - [Example:](#example)
    - [Pros:](#pros)
    - [Cons:](#cons)
    - [Use Cases:](#use-cases)
- [Google S2](#google-s2)
    - [How It Works:](#how-it-works)
    - [Key Features:](#key-features)
    - [Example:](#example)
    - [Pros:](#pros)
    - [Cons:](#cons)
    - [Use Cases:](#use-cases)
- [Comparison](#comparison)
- [Databases that can be used for Geospatial Data](#databases-that-can-be-used-for-geospatial-data)
    - [1. PostgreSQL + PostGIS](#1-postgresql--postgis)
    - [2. MySQL (InnoDB + Spatial Indexes)](#2-mysql-innodb--spatial-indexes)
    - [3. MongoDB (Geospatial Indexes)](#3-mongodb-geospatial-indexes)
    - [4. Redis (Geospatial Indexing)](#4-redis-geospatial-indexing)
    - [5. Google BigQuery (GIS Functions)](#5-google-bigquery-gis-functions)
    - [Which One Should You Use?](#which-one-should-you-use)
- [Reference](#reference)

# Some Basics

### What is Geospatial Data?

[**What is Geospatial Data?**](https://youtu.be/zp6Q4FMamqk)

[**What is Geolocation?**](https://youtu.be/6jdUqx1ax3o)

Geospatial data refers to information about objects, events, or phenomena that have a location on the Earth’s surface. Examples include:

- Latitude and longitude coordinates (e.g., 37.7749° N, 122.4194° W for San Francisco).
- Points of interest (e.g., restaurants, gas stations).
- Routes, boundaries, or regions (e.g., city boundaries, delivery zones).

When designing systems like Uber, you need to efficiently store, query, and process this geospatial data. This is where **geospatial indexing** and **spatial partitioning** come into play.

1. Indexing of geospatial data (2 ways)
    1. Hash
        1. Even Grid
        2. GeoHash
        3. Cartesian Tries
    2. Tree
        1. QuadTree
        2. Google S2
        3. RTree

### What is Geospatial Indexing?

Geospatial indexing is a way to organize geospatial data so that it can be queried efficiently. For example:

- **Query:** Find all drivers within 5 km of a user’s location.
- **Query:** Find the nearest gas station to a given location.

Without indexing, you’d have to scan through all the data points to answer these queries, which is inefficient. Geospatial indexing helps reduce the search space by organizing data in a way that makes queries faster.

### What is Spatial Partitioning?

Spatial partitioning is a technique to divide a large space (like a map) into smaller, manageable regions. This helps in:

- Efficiently querying data (e.g., finding nearby drivers).
- Distributing data across servers (e.g., in a distributed system like Uber).

Common spatial partitioning techniques include:

- **Grid-based partitioning:** Divide the map into a grid of equal-sized cells.
- **Quadtrees:** A hierarchical partitioning method (more on this below).
- **Geohashing:** Encode locations into a string for easy indexing.

Now that you have some context, let’s dive into the **5 common algorithms/methods** used to find nearby businesses/riders (or any geospatial queries). These are:

1. **Two-Dimensional Search (Brute Force)**
2. **Evenly Divided Grid**
3. **Geohash**
4. **Quadtree**
5. **Google S2**

# Two-Dimensional Search (Brute Force)

### How It Works:

- This is the simplest approach. You store all the points (e.g., rider or business locations) in a list or database.
- To find nearby points, you calculate the distance between the query point (e.g., user’s location) and every other point in the dataset using the **Haversine formula** (for Earth’s spherical geometry).
- Return all points within a certain radius.

### Pros:

- **Simple to implement:** No complex data structures or algorithms.
- **Accurate:** You check every point, so you don’t miss any results.

### Cons:

- **Inefficient for large datasets:** The time complexity is **O(n)**, where `n` is the number of points. For millions of points, this becomes very slow.
- **Not scalable:** As the dataset grows, performance degrades.

### Use Cases:

- Small datasets (e.g., a few hundred points).
- Prototyping or testing.

# Evenly Divided Grid

### How It Works:

- Divide the map into a **fixed-size grid** (e.g., 1 km x 1 km cells).
- Assign each point (e.g., rider or business) to a grid cell based on its coordinates.
- To find nearby points:
    - Identify the grid cell containing the query point.
    - Search in that cell and its neighboring cells (since nearby points might be in adjacent cells).

### Pros:

- **Faster than brute force:** Reduces the search space to a few grid cells.
- **Simple to implement:** Easy to understand and code.

### Cons:

- **Fixed grid size:** If the grid cells are too large, you still end up searching many points. If they’re too small, you waste memory.
- **Edge cases:** Points near the edge of a cell might be close to the query point but missed if you don’t check neighboring cells.

### Use Cases:

- Medium-sized datasets.
- Applications where the data is evenly distributed (e.g., city-based services).

# Geohash

[**Geohash: Deep Intuitive Understanding**](https://youtu.be/UaMzra18TD8)

**Geohash of my home: https://geohash.softeng.co/tdr606we**

[**All You Need To Know About Geohash**](https://docs.quadrant.io/quadrant-geohash-algorithm)

[**What is Geohashing?**](https://www.pubnub.com/guides/what-is-geohashing/)

[**Geohash: the algorithm inside and out - Part 1**](https://youtu.be/vGKs-c1nQYU)

[**Geohash: coding from scratch - Part 2**](https://youtu.be/ASLPkpKKCw4)

### How It Works:

Geohash is a **hierarchical spatial indexing method** that converts latitude and longitude into a **base-32 alphanumeric string**. It recursively divides the Earth's surface into a grid, where each additional character in the geohash refines the precision.

### Encoding Process:

1. Start with a bounding box of the entire Earth (-90 to 90 latitude, -180 to 180 longitude).
2. Alternately divide the range of latitude and longitude in **binary**:
    - 1st bit: Split longitude (West = 0, East = 1).
    - 2nd bit: Split latitude (South = 0, North = 1).
    - Continue recursively, adding more bits for more precision.
3. Convert the resulting binary string to **base-32 encoding** (digits 0-9, letters b-z excluding a, i, l, o to avoid confusion).

### Example:

For **latitude = 37.7749, longitude = -122.4194 (San Francisco, CA):**

- Binary representation: `110011000110000011011001101110`
- Base-32 encoding: `9q8yy`

The geohash `"9q8yy"` represents a small rectangular area around San Francisco.

### Pros:

✅ **Fast lookup** using string prefix matching (e.g., `"9q8yy"` and `"9q8y"` share the same area).

✅ **Compact storage** (short strings instead of lat/lon floats).

✅ **Hierarchical indexing** allows adjusting precision (longer geohash = finer detail).

✅ **Efficient range queries** using **trie-based** search in databases.

### Cons:

❌ **Grid distortion**: Geohash cells are **not uniform** (rectangular, not square).

❌ **Edge cases**: Two nearby points could have very different geohashes if they are near a grid boundary.

❌ **Not great for k-nearest neighbors (kNN) queries** due to non-uniform partitions.

### Use Cases:

- **Databases**: Elasticsearch, PostgreSQL, and Redis use geohash for geospatial indexing.
- **Ride-hailing (Uber, Lyft, etc.)**: Efficiently filter drivers within a geohash prefix.
- **Geospatial Search**: "Find all restaurants near '9q8yy' geohash."

# Quadtree

[**QuadTrees and Hilbert Curves**](https://youtu.be/OcUKFIjhKu0)

[**Quadtrees Explained with Code in Easiest Way**](https://youtu.be/kClPBNmTCf0)

[**Trees QuadTree OctTree**](https://youtu.be/XvISDt21lhA)

[**What is a Quadtree & how is it used in location-based services?**](https://www.educative.io/answers/what-is-a-quadtree-how-is-it-used-in-location-based-services)

### How It Works:

A **quadtree** is a **hierarchical spatial data structure** that recursively divides space into four quadrants (**NW, NE, SW, SE**).

### Structure:

1. Start with a **root node** that represents the entire world.
2. Recursively **subdivide into four equal-sized quadrants**.
3. Each quadrant is assigned a **region code** (`00, 01, 10, 11`).
4. Continue subdividing until each node contains a **small enough number of points** or reaches a **minimum size**.

### Example:

For a **city map**, a quadtree might look like:

```
Root (Whole City)
 ├── NW (Downtown)
 │   ├── NW (Main Street)
 │   ├── NE (High Street)
 │   ├── SW (Suburb)
 │   ├── SE (Mall)
 ├── NE (Airport)
 ├── SW (Industrial Zone)
 ├── SE (Residential)
```

Each level of subdivision increases **precision**.

### Pros:

✅ **Efficient spatial partitioning** (logarithmic lookup).

✅ **Adaptive refinement**: Dense areas get more subdivisions, sparse areas get fewer.

✅ **Fast kNN and range queries** by traversing relevant quadrants.

✅ **Works well with dynamic data** (adding/removing locations).

### Cons:

❌ **Can become unbalanced** if data is clustered (one quadrant gets too many points).

❌ **Overhead of recursive tree traversal** in some cases.

❌ **More complex than a simple grid or geohash.**

### Use Cases:

- **Uber H3**: Uber uses a hexagonal version of quadtrees for **efficient driver matching**.
- **Google Maps & GIS Systems**: Fast spatial indexing for rendering maps.
- **Game Development**: Used for spatial partitioning in large game worlds.

# Google S2

### How It Works:

Google S2 is an **advanced spatial indexing system** that maps Earth's surface onto a **unit sphere**, then projects it onto a **quad-tree-based coordinate system**.

### Key Features:

1. **Earth is projected onto a sphere** instead of a flat plane, avoiding distortion issues seen in Geohash/Quadtree.
2. **Each cell in S2 is part of a quadtree**, enabling hierarchical lookups.
3. **Cell IDs** are assigned based on a **Hilbert Curve**, ensuring **spatial locality** (nearby points have similar IDs).

### Example:

1. Earth is divided into **six face regions** using a **cube projection**.
2. Each face is subdivided into four quadrants recursively.
3. The result is a hierarchical spatial grid, with each cell having an ID like:
    - `Cell ID: 3748290102932`
    - Higher precision levels refine the search.

### Pros:

✅ **Better shape accuracy than geohash** (cells are more uniform).

✅ **Scales efficiently** for Google-scale applications.

✅ **Preserves locality** (cells are ordered spatially using a Hilbert Curve).

✅ **Used in Google Maps, Uber, and Foursquare** for real-world geospatial queries.

### Cons:

❌ **More complex than Geohash/Quadtree** (requires working with S2 libraries).

❌ **Slightly more computational overhead** compared to simpler methods.

### Use Cases:

- **Google Maps**: Used for location-based searches and geospatial queries.
- **Foursquare & Uber**: Used for rider-driver matching and check-in locations.
- **AI/ML**: Used in spatial data processing for machine learning applications.

# Comparison

| Method | Complexity | Space Efficiency | Scalability | Best For |
| --- | --- | --- | --- | --- |
| Two-Dimensional Search | O(N) | High | Low | Small datasets |
| Evenly Divided Grid | O(1) - O(k) | Medium | Moderate | Uniform data distribution |
| Geohash | O(log N) | High | Good | Approximate location search |
| Quadtree | O(log N) | Medium | High | Large-scale spatial partitioning |
| Google S2 | O(log N) | High | Very High | Google-scale applications |

| Feature | Geohash | Quadtree | Google S2 |
| --- | --- | --- | --- |
| **Data Structure** | String-based grid | Tree-based | Hierarchical quad-based |
| **Hierarchical?** | ✅ Yes | ✅ Yes | ✅ Yes |
| **Uniform Partitioning?** | ❌ No (rectangular) | ❌ No (adaptive) | ✅ Yes (balanced) |
| **Edge Case Handling** | ❌ Grid distortions | ❌ Can be unbalanced | ✅ Handles edges well |
| **Performance** | Good | Better | Best |
| **Ease of Use** | ✅ Simple | ⚠️ Medium | ❌ Complex |
| **Best For** | Databases, Approx. Search | Large-Scale Spatial Indexing | Google-Scale Applications |

# Databases that can be used for Geospatial Data

Several databases support geospatial data, each offering different indexing and query capabilities. Here's a breakdown of the most commonly used databases for storing and querying geospatial data.

### 1. PostgreSQL + PostGIS

**Why Use It?**

PostgreSQL with the **PostGIS** extension is one of the most powerful open-source databases for handling geospatial data. PostGIS adds support for spatial data types and spatial indexes.

**How to Store Geospatial Data in PostgreSQL?**

1. **Install PostGIS Extension:**
    
    ```sql
    CREATE EXTENSION postgis;
    ```
    
2. **Create a Table with Geospatial Columns:**
    
    ```sql
    CREATE TABLE locations (
        id SERIAL PRIMARY KEY,
        name TEXT,
        geom GEOMETRY(Point, 4326) -- WGS 84 coordinate system
    );
    ```
    
3. **Insert Geospatial Data:**
    
    ```sql
    INSERT INTO locations (name, geom)
    VALUES ('Golden Gate Bridge', ST_GeomFromText('POINT(-122.4783 37.8199)', 4326));
    ```
    
4. **Create a Spatial Index (for faster queries):**
    
    ```sql
    CREATE INDEX idx_locations_geom ON locations USING GIST(geom);
    ```
    
5. **Find Locations Near a Point (e.g., within 10 km):**
    
    ```sql
    SELECT name
    FROM locations
    WHERE ST_DWithin(
        geom,
        ST_GeomFromText('POINT(-122.4194 37.7749)', 4326),
        10000
    );
    ```
    

**Pros:**

✅ Advanced spatial queries (e.g., distance, intersections, and nearest neighbors).

✅ **GiST & BRIN indexes** for efficient geospatial lookups.

✅ Open-source and widely supported.

Cons:

❌ Requires **PostGIS extension** for full functionality.

❌ More complex than NoSQL solutions for simple use cases.

Use Cases:

- Storing **addresses, cities, and landmarks** with geospatial queries.
- **Ride-hailing (Uber, Lyft)**: Matching riders with nearby drivers.
- **Real estate apps**: Finding properties within a region.

### 2. MySQL (InnoDB + Spatial Indexes)

**Why Use It?**

MySQL supports geospatial data using **SPATIAL indexes** but with **limited functions** compared to PostgreSQL.

**How to Store Geospatial Data in MySQL?**

1. **Create a Table with a Spatial Column:**
    
    ```sql
    CREATE TABLE places (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255),
        location POINT NOT NULL,
        SPATIAL INDEX (location)
    );
    ```
    
2. **Insert Data:**
    
    ```sql
    INSERT INTO places (name, location)
    VALUES ('Eiffel Tower', ST_GeomFromText('POINT(2.2945 48.8584)'));
    ```
    
3. **Find Nearby Locations (Bounding Box Search - Approximate):**
    
    ```sql
    SELECT name
    FROM places
    WHERE MBRContains(
        ST_GeomFromText('POLYGON((2.28 48.85, 2.31 48.85, 2.31 48.87, 2.28 48.87, 2.28 48.85))'),
        location
    );
    ```
    

**Pros:**

✅ Supports **SPATIAL INDEXES** for fast geospatial queries.

✅ **Simple to set up** for basic geospatial applications.

**Cons:**

❌ Limited spatial functions (no ST_DWithin, no full-featured GIS functions).

❌ **Bounding box** search (not true radius-based searches).

**Use Cases:**

- **Small-scale location-based apps** with basic distance filtering.
- **Geospatial-enabled business directories.**

### 3. MongoDB (Geospatial Indexes)

**Why Use It?**

MongoDB supports **geospatial indexing** for fast proximity searches in NoSQL environments.

**How to Store Geospatial Data in MongoDB?**

1. **Create a Collection and Insert Data:**
    
    ```
    db.places.insertOne({
        name: "Statue of Liberty",
        location: { type: "Point", coordinates: [-74.0445, 40.6892] }
    });
    ```
    
2. **Create a 2dsphere Index for Geospatial Queries:**
    
    ```
    db.places.createIndex({ location: "2dsphere" });
    ```
    
3. **Find Nearby Locations (Within 5 km Radius):**
    
    ```
    db.places.find({
        location: {
            $near: {
                $geometry: { type: "Point", coordinates: [-74.0445, 40.6892] },
                $maxDistance: 5000
            }
        }
    });
    ```
    

**Pros:**

✅ **Fast geospatial queries** with `2dsphere` indexes.

✅ Supports **GeoJSON format** for flexible location storage.

✅ NoSQL database - works well with dynamic schema.

**Cons:**

❌ Not as powerful as PostGIS for advanced GIS calculations.

❌ **Limited to spherical (Earth-like) distance calculations.**

**Use Cases:**

- **Ride-sharing apps (Uber, Lyft)** for driver-passenger proximity searches.
- **Food delivery services** for finding nearby restaurants.
- **Real-time tracking applications** (IoT, logistics, fleet tracking).

### 4. Redis (Geospatial Indexing)

**Why Use It?**

Redis provides **fast in-memory geospatial indexing**, making it ideal for **real-time location lookups**.

**How to Store Geospatial Data in Redis?**

1. **Add Locations to a Geospatial Set:**
    
    ```
    GEOADD places -74.0445 40.6892 "Statue of Liberty"
    GEOADD places -73.9857 40.7484 "Empire State Building"
    ```
    
2. **Find Nearby Locations (Within 10 km):**
    
    ```
    GEORADIUS places -74.0445 40.6892 10 km WITHDIST
    ```
    
3. **Get Distance Between Two Points:**
    
    ```
    GEODIST places "Statue of Liberty" "Empire State Building" km
    ```
    

**Pros:**

✅ **Blazing-fast lookups** (O(log N) performance).

✅ **Simple API** for basic geospatial searches.

✅ **Great for caching location data** (e.g., frequent nearby searches).

**Cons:**

❌ No advanced geospatial functions (e.g., polygons, intersections).

❌ **Not persistent** (best for short-term storage).

**Use Cases:**

- **Real-time ride-hailing systems (Uber, Lyft, Ola).**
- **Fast lookups of nearby locations** in a high-traffic app.
- **Cache layer for more powerful geospatial databases.**

### 5. Google BigQuery (GIS Functions)

**Why Use It?**

BigQuery supports **geospatial analytics at scale**, making it useful for **large-scale location-based analysis**.

**How to Store Geospatial Data in BigQuery?**

1. **Create a Table with a Geography Column:**
    
    ```sql
    CREATE TABLE my_dataset.places (
        id STRING,
        name STRING,
        location GEOGRAPHY
    );
    ```
    
2. **Insert Data:**
    
    ```sql
    INSERT INTO my_dataset.places (id, name, location)
    VALUES ('1', 'Central Park', ST_GEOGPOINT(-73.968285, 40.785091));
    ```
    
3. **Find Places Within a Radius:**
    
    ```sql
    SELECT name
    FROM my_dataset.places
    WHERE ST_DWithin(
        location,
        ST_GEOGPOINT(-73.9857, 40.7484),
        5000
    );
    ```
    

**Pros:**

✅ Handles **massive geospatial datasets**.

✅ **SQL-based GIS functions** similar to PostGIS.

**Cons:**

❌ **Expensive for small-scale applications.**

❌ **Best for batch processing, not real-time queries.**

### Which One Should You Use?

| Database | Best For |
| --- | --- |
| **PostGIS (PostgreSQL)** | Complex geospatial queries, mapping |
| **MySQL** | Basic geospatial storage |
| **MongoDB** | NoSQL + geospatial queries |
| **Redis** | Fast, real-time location lookups |
| **BigQuery** | Large-scale geospatial analytics |

# Reference

1. [**System Design: Quadtrees & GeoHash**](https://towardsdatascience.com/system-design-quadtrees-geohash-5b896b975262/)
2. [**Quadtree vs Geohash? What to choose when designing your next Uber/Yelp**](https://levelup.gitconnected.com/design-a-proximity-service-like-uber-yelp-understanding-the-spatial-indexing-techniques-9cb18a05ad0d)
3. [**Geospatial | GeoHash | R-Tree | QuadTree**](https://tarunjain07.medium.com/geospatial-geohash-notes-15cbc50b329d)
4. [**Geospatial Indexing Explained: A Comparison of Geohash, S2, and H3**](https://benfeifke.com/posts/geospatial-indexing-explained/)
