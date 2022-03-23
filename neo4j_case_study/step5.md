# Simple database queries

Now that the data is loaded, you can query the database using the Cypher Query Language (CQL). If you wanna know more about the syntax of CQL and learn how to write you check the the offical neo4j documentation [here](https://neo4j.com/developer/cypher/). For the scope of this scenario, it is enough that you can execute some example queries.

To check how many nodes of each label there are, you can execute the following command:  
```
MATCH (n) RETURN count(labels(n)), labels(n);
```{{execute}}



Get information about the movie "The Matrix":  
```
MATCH (movie:Movie)-[relationship]-(node) 
WHERE movie.name = "The Matrix" 
RETURN movie.name, relationship, node.name
ORDER BY type(relationship);
```{{execute}}
The output of this query is probably confusing in the terminal. This is because the output is a graph and it can not be visualized in the command line. Here is how the output looks using the Neo4j Browser User Interface:

# TODO INSERT IMAGE OF GRAPH VISUALIZATION


Which Person has directed the most movies?
```
MATCH (director:Person)-[rel:DIRECTED]->(m:Movie)
WITH director, count(m) as total_movies, collect(m.id) as reference_list
RETURN director.name, total_movies, reference_list
ORDER BY total_movies DESC
LIMIT 5;
```{{execute}}

