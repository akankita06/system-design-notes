## Denormalized DB
- Fast reads and writes (schema less)
- Schema less - no validation on db layer so faster. Applications take care of validating data schema.
- Identify access patterns and model data for dynamo db around that
- Entire data collection required to an access pattern together

## Document DB
- indexes can be created on any field or sub-field of the document
- lot of data
- vast variety of attributes (differing from item to item)
- elasticsearch is a special case of document DB
- key-value pair
  - retrieve one record
  - Like map or dictionary


## Columnar DB
- retrieve multiple related records
- Like a 2d hash table, value of reach hash is a b tree
- Analogy - library - u forest define which section/ collection u want the book from ( say fiction). Within that collection the values are sorted based on an index. U can then fetch the record or take off records u want.
![Columnar DB](images/columnarDB.png)


## Cassandra
