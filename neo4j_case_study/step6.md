# Advanced database queries: Movie recommendations

idea: same genre, same director or actors
## Movies with shared genre, director, and actors

```
MATCH (the_matrix:Movie {name: "The Matrix"})-[:LISTED_IN]->(genre:Genre)<-[:LISTED_IN]-(recommendation:Movie)
WHERE the_matrix <> recommendation 
WITH DISTINCT the_matrix, recommendation, collect(DISTINCT genre.name) as shared_genres
RETURN the_matrix.name, recommendation.name, shared_genres
ORDER BY size(shared_genres) DESC
LIMIT 10;
```{{execute}}

MATCH (the_matrix:Movie {name: "The Matrix"})-[:LISTED_IN]->(genre:Genre)<-[:LISTED_IN]-(recommendation:Movie)<-[:ACTED_IN]-(actor:Person)-[:ACTED_IN]->(the_matrix)
WHERE the_matrix <> recommendation 
WITH DISTINCT the_matrix, recommendation, collect(DISTINCT genre.name) as shared_genres, collect(DISTINCT actor.name) as shared_actors 
RETURN the_matrix.name, recommendation.name, shared_genres, shared_actors;

## Using Graph algorithms
Adamic Adar is a measure used to compute the closeness of nodes based on their shared neighbors. https://neo4j.com/docs/graph-data-science/current/alpha-algorithms/adamic-adar/

```
MATCH (matrix:Movie {name: "The Matrix"} )-[*2]-(recommendation:Movie)
WHERE matrix <> recommendation
WITH DISTINCT matrix, recommendation
RETURN matrix.name as Source, recommendation.name as Recommendation, gds.alpha.linkprediction.adamicAdar(matrix, recommendation) AS closeness_score
ORDER BY closeness_score DESC
LIMIT 15;
```{{execute}}

```
MATCH (matrix:Movie {name: "Rocky"} )-[*2]-(recommendation:Movie)
WHERE matrix <> recommendation
WITH DISTINCT matrix, recommendation
RETURN matrix.name as Source, recommendation.name as Recommendation, gds.alpha.linkprediction.adamicAdar(matrix, recommendation) AS closeness_score
ORDER BY closeness_score DESC
LIMIT 15;
```{{execute}}