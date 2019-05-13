## Neo4j Docker

### About Neo4j <br>
We use Neo4j on Assignment 5 to learn about graph databases. We needed to
install it on our computers and run it using the browser. Instead of adding more
software on my system, I chose to use a Docker container running Neo4j. The
official Neo4j Docker setup guide is very helpful with this. 

[Their Guide](https://neo4j.com/developer/docker-run-neo4j/)

### Organizing The Data To Be Imported<br>
In the assignment we must modify our Neo4j configuration to allow for data to be
imported from anywhere. Instead of modifying the configuration, I just copied
the data to the import folder that we mapped in the previous step. To do so,
copy the given `parts` folder to `neo4j/imports/` Where `neo4j` is
the directory you created in the previous step. 

#### Showing Where The Folder Is <br>

![Showing where the files are put](images/neo4j1.png){:class="img-responsive"}

### Importing The Data <br>
We were given instructions on how to import the data, but because the data is in
a different location, we need to change the instructions. The portion of the
command we need to modify:
<br> 
```shell
LOAD CSV WITH HEADERS FROM 'file:///C:/363SPR2019/parts/parts.csv'
```
<br>
Instead we change it to: <br>
```shell
LOAD CSV WITH HEADERS FROM 'file:///parts/parts.csv'
```
<br>
The other 2 imports need to be modified in the same way. 

### Doing The Assignment <br> 
While doing the assignment we used the Neo4j Browser. To access this, navigate
to `localhost:7474` and use the editor. There you will write and
run your queries.
