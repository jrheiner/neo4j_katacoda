# Advanced database queries: Movie recommendations

idea: same genre, same director or actors
## Movies with shared genre, director, and actors
```
MATCH (the_matrix:Movie {name: "The Matrix"})-[:IN_GENRE]->(genre:Genre)<-[:IN_GENRE]-(recommendation:Movie)<-[:DIRECTED]-(director:Person)-[:DIRECTED]->(the_matrix) WHERE the_matrix <> recommendation WITH DISTINCT the_matrix, recommendation, director, collect(DISTINCT genre.name) as shared_genres RETURN the_matrix.name, recommendation.name, shared_genres, director.name;
```
{execute}

## Using Graph algorithms
```
MATCH (a:Game {name: "Half-Life"} )-[*2]-(b:Game)
WHERE a <> b
WITH DISTINCT a,b
RETURN a.name as title, b.name as recommendation, gds.alpha.linkprediction.adamicAdar(a, b) AS score
ORDER BY score DESC
LIMIT 10;
```
{execute}
https://neo4j.com/docs/graph-data-science/current/alpha-algorithms/adamic-adar/


```
MATCH (game:Game)-[:IN_GENRE]->(genre)
WITH {item:id(genre), categories: collect(id(game))} AS userData
WITH collect(userData) AS data
CALL gds.alpha.similarity.overlap.stream({data: data})
YIELD item1, item2, count1, count2, intersection, similarity
RETURN gds.util.asNode(item1).name AS from, gds.util.asNode(item2).name AS to,
       count1, count2, intersection, similarity
ORDER BY similarity DESC;
```
{execute}
https://neo4j.com/docs/graph-data-science/current/alpha-algorithms/overlap/


idea: algorithm to determine node similarity
