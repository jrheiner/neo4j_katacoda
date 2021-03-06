# neo4j_katacoda

[![](http://shields.katacoda.com/katacoda/jrheiner/count.svg)](https://www.katacoda.com/jrheiner "Get your profile on Katacoda.com")

This is an interactive Katacoda scenario, you can find it here: https://katacoda.com/jrheiner/scenarios/neo4j_case_study

Alternatively, visit https://www.katacoda.com/jrheiner to view the profile.

## Neo4j case study: Movie recommendations
Neo4j is an open-source, NoSQL, native graph database. A graph database stores nodes and relationships instead of tables or documents. https://neo4j.com/developer/graph-database/


This Katacoda guides you through creating a graph model for a given dataset, loading data from a CSV file, and utilizing the graph database with different queries. The dataset is sourced from [Kaggle](https://www.kaggle.com/shivamb/netflix-shows) and contains Netflix titles. The original dataset was reduced in size to minimize the loading/computing times in this scenario.

The goal of the case study is to import the CSV dataset into the graph database with a suitable schema and perform database queries that allow giving movie recommendations. For example, a user recommendation based on the movie "The Matrix" could list the similar movies "The Matrix Reloaded" and "The Matrix Revolutions" as they are directed by the same people and share the genres and actors.