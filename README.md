Red Hat Microsweeper demo with Quarkus 
==========================

This demo uses a number of cloud technologies to implement a simple game from the earlier days of computing: Minesweeper!

![Screenshot](docs/microsweeper.png)

Technologies include:

* JQuery-based Minesweeper written by [Nick Arocho](http://www.nickarocho.com/) and [available on GitHub](https://github.com/nickarocho/minesweeper).
* Backend based on [Quarkus](https://quarkus.io) to persist scoreboard and provide a reactive frontend and backend connected to [Postgres](https://azure.microsoft.com/en-us/services/postgresql/).

-----------
# Deploy on OpenShift 

## Buildconfig
```
$ oc new-project microsweeper-buildconfig
$ oc adm policy add-scc-to-user anyuid -z default
$ oc new-app  -e POSTGRESQL_USER=microsweeper -e POSTGRESQL_PASSWORD=password1 -e POSTGRESQL_DATABASE=microsweeper postgresql:13-el7
$ oc new-app https://github.com/redhat-mw-demos/microsweeper-quarkus -e 'QUARKUS_DATASOURCE_JDBC_URL=jdbc:postgresql://postgresql/microsweeper?user=microsweeper&password=password1'
$ oc expose service/microsweeper-quarkus
```
## Pipeline Operator
```
$ oc new-project microsweeper-pipeline
$ oc create sa microsweeper
$ oc create is microsweeper-quarkus
$ oc adm policy add-scc-to-user anyuid -z microsweeper
$ oc apply -f deploy/pipeline.yml
```
