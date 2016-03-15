#### Example queries for Syriaca database

_Don't forget to add the necessary prefixes in Stardog!_

##### Basic query to find properties and values of database

```
SELECT DISTINCT ?property ?value
WHERE {
  ?s ?property ?value
  }
```


##### Query to find names of all resources in database

```
SELECT DISTINCT ?name
WHERE {
  ?s rdfs:label ?name`
  }
```

#### Query to find the description of a place

```
SELECT ?description
WHERE {
  <http://syriaca.org/place/78> dcterms:description ?description
  }
```

