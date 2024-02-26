# A Survey of MySQL Index Types

Why use indexes? without indexes:
- The entire table must be read
- Data may not be ordered
- Time-consuming

InnoDB indexes
- Indexes gain the ability to go to the desired location
- Clustered index: indexed by value
- MySQL uses B+ trees
- Records are stored by primary key; if there are no primary keys the engine will pick one
- Secondary keys: non pk used to access records in other tables, not nullable
- Avoid null in indexes
- Indexes are easy to create but they take space, add overhead for maintenance
- Indexes recommendation
    - INT/BIGINT
    - UNSIGNED
    - NOT NULL
    - AUTOINCREMENT
- Use `explain`, `explain format=tree`
- We should not index everything, but many things can be
- Search by first `n` characters: create index with `CREATE INDEX xxx ON table(field:n)`
- Spatial index to index spatial data
- Can add index on dates `CREATE INDEX xxx ON est_month_index(month(est_delivery)))`
- Can index calculations (eg `field * 1.5`
- Multi value indexes to index json
    - SELECT * FROM x WHERE y MEMBER OF (info->"$.zipcode")
- Multi-column: put rarest in the left. 3 columns indexes is valid for 1, 2 or 3 columns
- Hash joins: done automatically and showed in explain, good thing
- Invisible indexes: not seen by the optimizer, can be used to test efficiency of index
    - `ALTER TABLE x ALTER INDEX xx INVISIBLE`, then turn to `VISIBLE`
- Histogram: `ANALYZE TABLE x UPDATE HISTOGRAM ON x WITH 10 BUCKETS`
