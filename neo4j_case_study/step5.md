# Simple database queries

Now that the data is loaded, you can query the database using the Cypher Query Language (CQL). If you wanna know more about the syntax of CQL and learn how to write you check the the offical neo4j documentation [here](https://neo4j.com/developer/cypher/). For the scope of this scenario, it is enough that you can execute some example queries.

To check how many nodes of each label there are, you can execute the following command:  
```
MATCH (n) RETURN count(labels(n)), labels(n);
```{execute}


Get information about the movie "The Matrix":  
```
MATCH (movie:Movie)-[relation]->(node) WHERE movie.title = "The Matrix" RETURN movie, relation, node;
```{execute}
The output of this query is probably confusing in the terminal. This is because the output is a graph and it can not be visualized in the command line. Here is how the output looks using the Neo4j Browser User Interface:

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

