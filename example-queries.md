#### Example queries for Syriaca database

_Some things to remember:_
* Don't forget to add the necessary prefixes in Stardog - queries that use undefined language will return no results!
* URIs go between angle brackets (< >).
* Capitalization for query clauses (e.g. SELECT, WHERE, CONSTRUCT, etc.) is convention and not required.
* For the sake of example, [Edessa](http://syriaca.org/place/78/html) has been used as the query subject. The same can be done for other resources in [Syriaca](syriaca.org).
* Adding or removing the DISTINCT clause will greatly affect the number of results.

##### Query for the properties and values of database:

```
SELECT DISTINCT ?property ?value
WHERE {
  ?s ?property ?value
  }
```


##### Query for the labels of all resources in database:

```
SELECT DISTINCT ?label
WHERE {
  ?s rdfs:label ?label
  }
```

##### Query for the description of a place:

```
SELECT ?description
WHERE {
  <http://syriaca.org/place/78> dcterms:description ?description
  }
```

##### Query for the name(s) of a place:

```
SELECT ?name
WHERE {
  <http://syriaca.org/place/78> lawd:hasName ?name
  }
```

