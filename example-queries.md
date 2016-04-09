#### Example queries for Syriaca database (access Stardog the endpoint [here](http://dev-rdf.library.vanderbilt.edu/))

_Some things to remember:_
* Don't forget to add the necessary prefixes in Stardog - queries that use undefined language will return no results!
* URIs go between angle brackets (e.g. `<http://syriaca.org/place/78>`).
* Capitalization for query clauses (e.g. `SELECT`, `WHERE`, `CONSTRUCT`, etc.) is convention and _not_ required.
* Adding or removing the `DISTINCT` clause can greatly affect the number of results.
* Queries that are well formed but don't match any data will return empty (i.e. blank); there is no "no results" in SPARQL. (See "Query with optional bindings" below.)
* Variables in SPARQL (e.g. `?WHERE`) are arbitrary and therefore meaningless to the machine. They are simply meant to assist the human reader. The question mark (`?`) is what matters to the machine. You could - for example - use `?WAFFLES` to query for `rdfs:label`.
* Any query for data about a specific place can easily be transformed into a query about all places (and vice versa) simply by swapping out the URI (e.g. `<http://syriaca.org/place/78>`) for a variable (e.g. `?s`).
* An `ASK` query is a helpful way to explore what's in an unfamiliar dataset. However, it simply returns either `true` or `false` depending on if the requested data is in the dataset; it does not say how many, where, or what kind are used.


##### Query for properties and values:
```
SELECT DISTINCT ?property ?value
WHERE {
  ?s ?property ?value .
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

> The result is `"-0304"` (i.e. 304 BCE). We can use that to ask for places that have dates earlier than Edessa:

```
SELECT ?place ?placeName ?date
WHERE {
  ?place a lawd:Place ;
    rdfs:label ?placeName .
  ?place dcterms:temporal ?date .
  FILTER ( ?date > "-0304")
  }
ORDER BY ?date
```
> To find places with dates later than Edessa, simply invert the "greater than" (`>`) sign.


##### Query for a place's relations and their names:

```
SELECT DISTINCT ?related ?relName
WHERE {
  <http://syriaca.org/place/78> skos:related ?related .
  ?related rdfs:label ?relName .
  }
```


##### Query for related places and their names:
```
SELECT DISTINCT ?place ?placeName ?related ?relName
WHERE {
  ?place a lawd:Place ;
    rdfs:label ?placeName ;
    dcterms:relation ?related .
  ?related rdfs:label ?relName .
  }
```

##### Query with optional bindings:
```
SELECT DISTINCT ?label ?name ?primaryName ?variantName
WHERE { {
  <http://syriaca.org/place/1259> rdfs:label ?label ;
                                  lawd:hasName ?name ;
                                  lawd:hasName/lawd:primaryForm ?primary .
  }
OPTIONAL { <http://syriaca.org/place/1259> lawd:hasName/lawd:variantForm ?variant . }
  }
```
> `OPTIONAL` binding means that the query will not be invalidated if there is no match.
> Place 1259 (Hālānā) was used because it does not have `lawd:variantForm` in its data.


##### Query only for places that are related or a part of another place:
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
> NOTE: `dcterms:relation` and `skos:related` are different terms with different purposes - `skos:related` for things that are related in some broad sense, `dcterms:relation` for things that are related by name.


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
