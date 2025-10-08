Download and decompress the watdiv data:
```
wget https://dsg.uwaterloo.ca/watdiv/watdiv.10M.tar.bz2
tar -xvjf watdiv.10M.tar.bz2 -C ./data
```

Start up the container in the background:
```
 docker compose up -d
```

Load data by entering the containers interactive SQL and SPARQL command-line client (ISQL):
```
docker exec -it watdiv-virtuoso isql 1111 dba dba
```

Then run:
```
LD_DIR('/data', 'watdiv.10M.nt', 'http://watdiv.org');
rdf_loader_run();
checkpoint();
SPARQL SELECT (COUNT(*) AS ?triples) WHERE { GRAPH <http://watdiv.org> { ?s ?p ?o } };
exit;
```
to load `watdiv.10M.nt` into the graph `http://watdiv.org`.

The database will be stored in `./database/virtuoso.db` on the host and will persist even if the Virtuoso container is removed.