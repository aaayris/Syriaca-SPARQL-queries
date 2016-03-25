#### Example queries for Syriaca database (access Stardog endpoint [here](http://dev-rdf.library.vanderbilt.edu/))

_Some things to remember:_
* Don't forget to add the necessary prefixes in Stardog - queries that use undefined language will return no results!
* URIs go between angle brackets (e.g. `<http://syriaca.org/Places/78.html>`).
* Capitalization for query clauses (e.g. `SELECT`, `WHERE`, `CONSTRUCT`, etc.) is convention and _not_ required.
* For the sake of example, [Edessa](http://syriaca.org/place/78/html) has been used as the query subject (when specified). Other resources in [Syriaca](syriaca.org) can be used in the same way.
* Adding or removing the `DISTINCT` clause will greatly affect the number of results.
* Queries that are well formed but don't match any data will return empty (i.e. blank); there is no "no results" in SPARQL.
* Variables in SPARQL (e.g. `?where`) are arbitrary and therefore meaningless. They are simply meant to assist the human reader. The question mark (`?`) is what matters to the machine. You could - for example - use `?waffles` to query for `rdfs:label`.

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


##### Query for the location of a place:

```
SELECT ?location
WHERE {
  <http://syriaca.org/place/78> geo:location ?location
  }
```


##### Query for geographic relations:

```
SELECT ?partOf
WHERE {
  <http://syriaca.org/place/78> dcterms:isPartOf ?partOf
  }
```


##### Query for the date associated with a place:

```
SELECT ?date
WHERE {
  <http://syriaca.org/place/78> dcterms:temporal ?date
  }
```


##### Query for related places:

```
SELECT DISTINCT ?name ?related
WHERE {
  ?s rdfs:label ?name .
  ?s dcterms:relation ?related .
  }
```


##### Query for only places that are related or a part of another place:

```
SELECT DISTINCT *
WHERE {
  { ?s a lawd:Place ;
    rdfs:label ?label ;
    dcterms:relation ?rel .
  ?rel rdfs:label ?relName . }
UNION 
  { ?s a lawd:Place ;
    rdfs:label ?label ;
    dcterms:isPartOf ?partOf .
    ?partOf rdfs:label ?partOfName . }
}
```
> NOTE: When this query is run on the data, Edessa (place 78) is the only resource with dcterms:isPartOf in its definition; no other place is currently defined with the same namespace.
