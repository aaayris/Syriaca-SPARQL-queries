#### Example queries for Syriaca database (access Stardog endpoint [here](http://dev-rdf.library.vanderbilt.edu/))

_Some things to remember:_
* Don't forget to add the necessary prefixes in Stardog - queries that use undefined language will return no results!
* URIs go between angle brackets (e.g. `<http://syriaca.org/Places/78>`).
* Capitalization for query clauses (e.g. `SELECT`, `WHERE`, `CONSTRUCT`, etc.) is convention and _not_ required.
* For the sake of example, [Edessa](http://syriaca.org/place/78/html) has been used as the query subject (when specified). Other resources in [Syriaca](syriaca.org) can be used in the same way.
* Adding or removing the `DISTINCT` clause will greatly affect the number of results.
* Queries that are well formed but don't match any data will return empty (i.e. blank); there is no "no results" in SPARQL.
* Variables in SPARQL (e.g. `?where`) are arbitrary and therefore meaningless to the machine. They are simply meant to assist the human reader. The question mark (`?`) is what matters to the machine. You could - for example - use `?waffles` to query for `rdfs:label`.
* Any query for data about a specific place can easily be transformed into a query about all places (and vice versa) simply by swapping out the URI (e.g. `<http://syriaca.org/place/78>`) for a variable (e.g. `?s`).
* `ASK` queries are a helpful way to explore what's in an unfamiliar dataset. However, they simply return either `true` or `false` depending on if the requested data is in the dataset; it does not say how many, where, or what kind are used.

##### Query for properties and values:

```
SELECT DISTINCT ?property ?value
WHERE {
  ?s ?property ?value
  }
```


##### Query for the English label of a place:

```
SELECT DISTINCT ?label
WHERE {
  <http://syriaca.org/place/78> rdfs:label ?label .
  FILTER (lang(?label)="en")
}
```


##### Query for a place description:

```
SELECT ?description
WHERE {
  <http://syriaca.org/place/78> dcterms:description ?description
  }
```


##### Query for a place name:

```
SELECT ?name
WHERE {
  <http://syriaca.org/place/78> lawd:hasName ?name
  }
```


##### Query for place location:

```
SELECT ?location
WHERE {
  <http://syriaca.org/place/78> geo:location ?location
  }
```


##### Query for geographic hierarchy:

```
SELECT ?partOf
WHERE {
  <http://syriaca.org/place/78> dcterms:isPartOf ?partOf
  }
```


##### Query for the date(s) associated with a place:

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



##### Using `ASK` queries to explore the dataset:
```
ASK {
  <http://syriaca.org/place/78> dcterms:date ?o .
}
```
> This will return `false` since `dcterms:date` is not used with `<http://syriaca.org/place/78>`. However, if you run the same query with a different namespace...

```
ASK {
  <http://syriaca.org/place/78> dcterms:description ?o .
}
```
> ...this returns `true` because `dcterms:description` is used in the data for Edessa (place 78), whereas `dcterms:date` is not.



##### Query for the latitude and longitude of places:
```
SELECT DISTINCT *
WHERE {
  ?s a lawd:place ;
    rdfs:label ?label ;
    geo:location/geo:lat ?lat ;
    geo:location/geo:long ?long .
}
```
