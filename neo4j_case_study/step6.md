# Advanced database queries: Movie recommendations

In the last step, you executed some simple queries. However, CQL can also be used to build complex queries.  

This case study aims to generate movie recommendations based on the graph database. Below are two approaches to build a query that returns movie recommendations.


## 1. Movies in the same genre
A simple idea to make a movie recommendation is to look for other movies in the same genre. If a user liked an action movie, another movie in the action genre seems like a valid recommendation.  

Using the query below, we can find five movies listed in the same genre as "The Matrix". The query works by selecting all movies nodes that have a relationship with the same genre, the "The Matrix" node has a relationship with.
```
MATCH (the_matrix:Movie {name: "The Matrix"})-[:LISTED_IN]->(genre:Genre)<-[:LISTED_IN]-(recommendation:Movie)
WHERE the_matrix <> recommendation 
WITH DISTINCT the_matrix, recommendation, collect(DISTINCT genre.name) as shared_genres
RETURN the_matrix.name as Source, recommendation.name as Recommendation, shared_genres
LIMIT 5;
```{{execute}}

However, while the query returns five movies that share genres with "The Matrix", there is no way to order the recommendations. Some movies might be more relevant than others. Finally, the recommendations do not include the sequel movies "The Matrix Reloaded" and "The Matrix Revolutions" which seem like obvious choices.


## 2. Using Graph algorithms
*Adamic Adar* is a graph algorithm that introduces a measure used to compute the closeness of nodes based on their shared neighbors. A value of zero indicates that two nodes are not close, while higher values indicate nodes are closer. ("Adamic Adar - Neo4j Graph Data Science", 2022; Adamic & Adar, 2003)


Using the graph algorithm, we can query the top 5 "closest" nodes to the "The Matrix" movie node. (Claudel, 2020)
```
MATCH (matrix:Movie {name: "The Matrix"} )-[*2]-(recommendation:Movie)
WHERE matrix <> recommendation
WITH DISTINCT matrix, recommendation
RETURN matrix.name as Source, recommendation.name as Recommendation, gds.alpha.linkprediction.adamicAdar(matrix, recommendation) AS closeness_score
ORDER BY closeness_score DESC
LIMIT 5;
```{{execute}}
This result seems very promising. Unsurprisingly, the sequel movies are among the closest nodes. This makes sense because they share a lot of genres, actor, and director nodes.

Obviously, the query is not limited to "The Matrix" but works with any movie in the dataset. Using the same query except changing the movie to "Rocky", returns recommendations based on the "Rocky" movie. Once again, the sequels are among the closest nodes. Try it with the command below:
```
MATCH (matrix:Movie {name: "Rocky"} )-[*2]-(recommendation:Movie)
WHERE matrix <> recommendation
WITH DISTINCT matrix, recommendation
RETURN matrix.name as Source, recommendation.name as Recommendation, gds.alpha.linkprediction.adamicAdar(matrix, recommendation) AS closeness_score
ORDER BY closeness_score DESC
LIMIT 5;
```{{execute}}
