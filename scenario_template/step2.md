# Neo4j docker container

Start Neo4j as docker container:
```
docker run -d --name neo4j_games -v ~/data:/import --env NEO4J_AUTH=neo4j/katacoda -p7474:7474 -p7687:7687 neo4j:4.4.4
docker exec -it neo4j_games bash
```{{execute}}

Start the Neo4j shell:
`cypher-shell -u neo4j -p katacoda`{{execute}}
