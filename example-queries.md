#### Example queries for Syriaca database

_Some things to remember:_
* Don't forget to add the necessary prefixes in Stardog - queries that use undefined language will return no results!
* URIs go between angle brackets (`< >`).
* Capitalization for query clauses (e.g. `SELECT`, `WHERE`, `CONSTRUCT`, etc.) is convention and not required.
* For the sake of example, [Edessa](http://syriaca.org/place/78/html) has been used as the query subject (when specified). Other resources in [Syriaca](syriaca.org) can be used in the same way.
* Adding or removing the `DISTINCT` clause will greatly affect the number of results.
* Question words (e.g. `?where`) are arbitrary and therefore meaningless. They are simply meant to assist the human reader. The question mark (`?`) is what matters to the machine. You could - for example - use `?waffles` to query for `rdfs:label`.

##### Query for the properties and values of the database:

```
SELECT DISTINCT ?property ?value
WHERE {
  ?s ?property ?value
  }
```


##### Query for the labels of all resources in the database:

```
SELECT DISTINCT ?label
WHERE {
  ?s rdfs:label ?label
  }
```


##### Query for the description of every place in the database:

```
SELECT ?place ?description
WHERE {
  ?place dcterms:description ?description
  }
```


##### Query for the description of a specific place:

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

