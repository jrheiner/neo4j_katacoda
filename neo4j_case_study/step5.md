# Simple database queries

Now that the data is loaded, you can query the database using the Cypher Query Language (CQL).

To check how many nodes of each label there are, you can execute the following command:  
```
MATCH (n) RETURN count(labels(n)), labels(n);
```{execute}


Get information about the movie "The Matrix":  
```
MATCH (movie:Movie)-[relation]->(node) WHERE movie.title = "The Matrix" RETURN movie, relation, node;
```{execute}
# TODO INSERT IMAGE OF GRAPH VISUALIZATION

Get the top 10 rated movies from the dataset:  
```
MATCH (movie:Movie) RETURN movie.title, movie.rating ORDER BY movie.rating DESC LIMIT 10;
```{execute}


Which Person has directed the most movies?
```
MATCH (director:Person)-[rel:DIRECTED]->(m:Movie)
WITH director, count(m) as total_movies
RETURN director.name, total_movies
ORDER BY total DESC
LIMIT 20;
```{execute}

https://neo4j.com/developer/cypher/