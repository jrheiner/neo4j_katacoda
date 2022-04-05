# Starting Neo4j as a docker container

To use the Neo4j database, start a docker container with the command below:
```
docker run -d --name neo4j_games -v ~/data:/import --env NEO4J_AUTH=neo4j/katacoda --env NEO4JLABS_PLUGINS='["graph-data-science"]' neo4j:4.4.4
docker exec -it neo4j_games bash
```{{execute}}

This starts a container with the `neo4j` image and automatically installs the `graph-data-science` plugin. The plugin will be used later on to utilize a graph algorithm. The parameter `-v ~/data:/import` mounts a volume so that the dataset file can be accessed within the container. `--env` sets environment variables.

If you see the prompt `root@...:/var/lib/neo4j#` in the terminal, you can proceed with the command below.
`cypher-shell -u neo4j -p katacoda`{{execute}}  
This will start the command-line tool used to interact with a neo4j instance.   


If you see the red console output "Connection refused", wait ten seconds and try the command again. This happens when the database is still initializing and not fully started. If you see `neo4j@neo4j>`, you can continue to the next step.