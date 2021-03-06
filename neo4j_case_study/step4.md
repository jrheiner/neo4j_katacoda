# Importing data from a CSV file
Now that the Neo4j database is running, you can import the dataset with the graph data model created in step 2.  

To import the dataset into the database, execute the following command. Don't worry; the command is explained on this page. But since the data import can take a moment to fully complete, you should start it now and then continue reading below.

```
CREATE CONSTRAINT personNameConstraint FOR (person:Person) REQUIRE person.name IS UNIQUE;

LOAD CSV WITH HEADERS FROM 'file:///netflix_titles.csv' AS row
CREATE (movie:Movie {id: row.show_id, name: row.title})
WITH movie, row
UNWIND split(row.director, ',') AS dir
MERGE (director:Person {name: trim(dir)})
MERGE (director)-[:DIRECTED]->(movie)
WITH movie, row
UNWIND split(row.cast, ',') AS act
MERGE (actor:Person {name: trim(act)})
MERGE (actor)-[:ACTED_IN]->(movie)
WITH movie, row
UNWIND split(row.listed_in, ',') AS gen
MERGE (genre:Genre {name: trim(gen)})
MERGE (movie)-[:LISTED_IN]->(genre);
```{{execute}}


To understand what the above command does in detail, let's look at the different parts:

1. `CREATE CONSTRAINT` creates a constraint that ensures that every node with the person label (actors and directors) will have a unique name property. It also implicitly creates an index. By indexing the name property, node lookup (e.g., MERGE) will be much faster. As a result, the import is significantly faster.

2. `LOAD CSV WITH HEADERS FROM 'file:///netflix_titles.csv' AS row` loads the CSV file from the `~/neo4j/data` directory in the Docker container and parses the single lines. `AS row` saves the values of a single line in the variable `row` so we can access them in the rest of the query. ("Cypher Query Language - Developer Guides", 2022)

3. `CREATE (movie:Movie {id: row.show_id, name: row.title})` creates a single node with "Movie" label and sets the properties `name` and `id` using the data from the CSV line.  ("Cypher Query Language - Developer Guides", 2022)

4. `UNWIND split(row.director, ',') as d` splits a string containing multiple directors into single variables. This way, we can create a single node for each director when a single CSV line contains multiple. For example, `"Lilly Wachowski, Lana Wachowski"` is split into `"Lilly Wachowski"` and `"Lana Wachowski"`. ("Cypher Query Language - Developer Guides", 2022)

5. `MERGE (director:Person {name: d})` creates a node with the "Person" label. However, different to the `CREATE`, `MERGE` checks if the node already exists. For instance, if the same person directs two movies, we only want to add a single node for that person. This is why `MERGE` is used here instead of `CREATE`. ("Cypher Query Language - Developer Guides", 2022)

6. `MERGE (director)-[:DIRECTED]->(movie)` creates a relationship between a single director node and the movie node that was created in bullet point 2. Like before, we use the `MERGE` command to only create the relationship if it does not exist. Therefore, we avoid duplicate relationships representing the same information. ("Cypher Query Language - Developer Guides", 2022)

The commands from 4, 5, and 6 are repeated to add nodes and relationships for all actors and genres of a movie. This is done for each line in the CSV file.

As a result, we have imported the data following the data model designed in step 2. Using the Neo4j Browser User Interface allows generating a visualization of the database model. Since the Katacoda terminal is limited to text, you can find the image output for the current database below. 

![Neo4j movies data model](./assets/movie_data_model.png)

You can see three node labels: `Movie`, `Person`, and `Genre`. The nodes are connected with three different relationships. A person node can represent a director or actor depending on which relationship is used.
