##### Basics of ElasticSearch

###### _index
Where the document lives (Like database name in SQL)

###### _type
The class of object that the document represents (Like table in SQL)

###### _id
The unique identifier for the document (Like row in SQL)


#### Indexing a Document:

Create a new document or replace an existing document<br>

1.Using your  own id: <br>
````
PUT /{_index}/{_type}/{_id}<br>
````

2.Autogenerating Id’s <br>
````
POST /{_index}/{_type}/ <br>
````

####  Retrieving a Document

1. Fetching the entire document<br>
````
GET /{_index}/{_type}/{_id}?pretty <br>
````

2.Partially retrieving a document<br>
````
GET /{_index}/{type}/{id}?_source=fieldName1,fieldName2<br>
````

3. Fetch all indices<br>
````
GET /_cat/indices?v <br>
````

4.Search for all Documents<br>
````
GET /_mapping?pretty=true <br>
````

5.Check if a Documents exists
````
GET /{_index}/{type}/{id}
````
It will return the document
With found tag=true and will return the entire document<br>


6.check whether a document exists—you’re not interested in the content
````
HEAD /{_index}/{type}/{id}
````

####  Updating a Document

**Note**: Documents in Elasticsearch are immutable; we cannot change them. 
Instead, if we need to update an existing document, we reindex or replace it<br>

````
PUT /{_index}/{type}/{id}
````
In the response, we can see that Elasticsearch has incremented the _version number:
The created flag is set to false because a document with the same index, type, and ID already existed.
Internally, Elasticsearch has marked the old document as deleted and added an entirely new document. 
The old version of the document doesn’t disappear immediately, although you won’t be able to access 
it. Elasticsearch cleans up deleted documents in the background as you continue to index more data.<br>

````
POST /{_index}/{type}/{id}_update
````
the update API simply manages the same retrieve-change-reindex process that we have already 
described. The difference is that this process happens within a shard, thus avoiding the 
network overhead of multiple requests. By reducing the time between the retrieveand reindex 
steps, we also reduce the likelihood of there being conflicting changes from other processes.
Objects are merged together, existing scalar fields are overwritten, and new fields are added. <br>

####  Creating a Document

1.Create a document only if the document does not already exist.
````
POST /{_index}/{type}/
````

2.If you have id.
````
PUT /{index}/{type}/{id}?op_type=create
````

####  Deleting a Document
```
DELETE /{index}/{type}/{id}
````