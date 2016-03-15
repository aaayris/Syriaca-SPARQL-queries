##### Example queries for Syriaca database

###### Basic query to find properties and values of database

```
select distinct ?property ?value
where {
  ?s ?property ?value
  }
```


###### Query to find names of all resources in database

```
select distinct ?name
where {
  ?s rdfs:label ?name`
  }
```
