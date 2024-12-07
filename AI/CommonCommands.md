- **MATCH (n) DETACH DELETE n;**

    To completely clear all data from your Neo4j database, you can use the following Cypher query:

    ```cypher
    MATCH (n)
    DETACH DELETE n;
    ```

    This command matches all nodes `(n)` in the database and uses `DETACH DELETE` to remove each node along with its relationships. 

- **Using Cypher in Neo4j 4.x Enterprise Edition:**

    Switch to the system database and execute the following commands:

        ```cypher
        DROP DATABASE yourDatabaseName;
        CREATE DATABASE yourDatabaseName;
        ```

    Replace `yourDatabaseName` with the name of your database