## Example queries for Syriaca database

### Basic Exploratory Query to Find Properties and Values of Database

`select ?property ?value`

`where {`

`  ?s ?property ?value`
  
`  }`


### Query to Find Names of All Resources in Database

`select distinct ?name`

`where {`

`  ?s rdfs:label ?name`

`}`
