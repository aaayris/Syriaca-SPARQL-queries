#### Example queries for Syriaca database

_Some things to remember:_
* Don't forget to add the necessary prefixes in Stardog - queries that use undefined language will return no results!
* URIs go between angle brackets (< >).
* Capitalization for query clauses (e.g. SELECT, WHERE, CONSTRUCT, etc.) is convention and not required.
* For the sake of example, [Edessa](http://syriaca.org/place/78/html) has been used as subject. The same can be done for other resources in [Syriaca](syriaca.org).

##### Basic query to find properties and values of database

```
SELECT DISTINCT ?property ?value
WHERE {
  ?s ?property ?value
  }
```


##### Query to find labels of all resources in database

```
SELECT DISTINCT ?label
WHERE {
  ?s rdfs:label ?label
  }
```

##### Query to find the description of a place

```
SELECT ?description
WHERE {
  <http://syriaca.org/place/78> dcterms:description ?description
  }
```

##### Query to find the name(s) of a place

```
SELECT ?name
WHERE {
  <http://syriaca.org/place/78> lawd:hasName ?name
  }
```

