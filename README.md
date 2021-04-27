# Couchbase


Couchbase Case için aşağıdaki kaynağı referans alarak kurulumlarıma başladım.
https://docs.couchbase.com/server/current/install/getting-started-docker.html
	Deploy a Multi-Node Cluster with Containers adımından itibaren devam edilmiştir.


![image](https://user-images.githubusercontent.com/53182424/116316390-8ad67c00-a7ba-11eb-973b-8c7bec213f32.png)

docker run -d --name db1 couchbase
docker inspect --format '{{ .NetworkSettings.IPAddress }}' db1
adımları 2.DB node için de tekrarlanır.
docker run -d --name db3 -p 8091-8096:8091-8096 -p 11210-11211:11210-11211 couchbase
bu adımla da db3 8091 portundan serve etmeye başlar.


![image](https://user-images.githubusercontent.com/53182424/116316422-945fe400-a7ba-11eb-9ee0-68b7e589bb98.png)

![image](https://user-images.githubusercontent.com/53182424/116316428-97f36b00-a7ba-11eb-9320-0099b5df118c.png)
