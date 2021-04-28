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

![image](https://user-images.githubusercontent.com/53182424/116316453-a3469680-a7ba-11eb-9a3c-9878aac75893.png)

Server Node ekleme adımı:

![image](https://user-images.githubusercontent.com/53182424/116316468-a9d50e00-a7ba-11eb-9e9e-aabaaf2d551c.png)

Rebalance:

![image](https://user-images.githubusercontent.com/53182424/116316484-b194b280-a7ba-11eb-81d0-f2374d4272ed.png)

Ejection method “full” seçilir.

![image](https://user-images.githubusercontent.com/53182424/116316502-b8bbc080-a7ba-11eb-8d3c-288dfb5647b7.png)

Bucket oluşturulup Yeni Doküman ID ile Json formatında select atıp cevap dönebilecek bir collection oluşturulur.

![image](https://user-images.githubusercontent.com/53182424/116316529-c2452880-a7ba-11eb-902a-f7d6545b5ea5.png)

XDCR ile nodular arasında replikasyon yapılır.

![image](https://user-images.githubusercontent.com/53182424/116316555-cbce9080-a7ba-11eb-9d67-93ae6ddeed99.png)

Full text search index eklenir. 

![image](https://user-images.githubusercontent.com/53182424/116316590-d9841600-a7ba-11eb-9667-fc784d30c022.png)

Diğer Node’dan dokümanın ulaşılabilir olduğu doğrulanır.

![image](https://user-images.githubusercontent.com/53182424/116316609-e1dc5100-a7ba-11eb-9c9b-04f6dbe53a22.png)

High availability için failover ve recover from Failover testleri yapılır.

![image](https://user-images.githubusercontent.com/53182424/116316643-ec96e600-a7ba-11eb-8098-45814975f7e2.png)

Add: Delta Back ve Rebalance adımıyla failover testi tamamlanır.
Items boyutları eşitlendiği görülebilir.

![image](https://user-images.githubusercontent.com/53182424/116316685-f7ea1180-a7ba-11eb-9b3a-a9477561731c.png)


Full Admin Yetkileriyle Node üzerinde bir index oluşturmak için:
#curl  -v -X POST http://172.17.0.4:8091/node/controller/setupServices -d 'services=kv%2Cn1ql%2Cindex'

![image](https://user-images.githubusercontent.com/53182424/116316710-02a4a680-a7bb-11eb-880f-bb6759b5ed6c.png)

Memory kotalarını ayarlamak  CLI üzerinden ayarlamak için:

![image](https://user-images.githubusercontent.com/53182424/116316731-0afce180-a7bb-11eb-891f-badefad02cec.png)

curl  -u Administrator:password -v -X POST \
http://10.142.181.101:8091/settings/web \
-d 'password=password&username=Administrator&port=SAME'


 Beklenen output:
{"newBaseUri":"http://10.142.181.101:8091/"}

![image](https://user-images.githubusercontent.com/53182424/116316765-1819d080-a7bb-11eb-99fb-6d0e0b76de21.png)


Edit Bucket thorugh CLI in case of needed situation
curl -v -X POST http://172.17.0.4:8091/pools/default/buckets/YEmre \
-u Administrator:password \
-d ramQuotaMB=512

![image](https://user-images.githubusercontent.com/53182424/116316831-2f58be00-a7bb-11eb-9724-f0aa2a459d74.png)


Ardından Python SDK kurulumlarını tamamladıktan sonra basic example ile kod (cb-test.py) içerisinde json tanımlanmış bilgiler getirildi.

Travel Sample adında bucket oluşturuldu.
 #python3 cb-test.py ile aşağıdaki sonuç elde edildi
 
 ![image](https://user-images.githubusercontent.com/53182424/116420199-6fb14e00-a846-11eb-87c2-8cebdd81e09d.png)


Olusturduğum dizinde ;

git clone https://github.com/couchbaselabs/try-cb-python.git

cd try-cb-python
git checkout 6.5
python3 -m pip install -r requirements.txt

branch'imi değiştirip .txt dosysını indiriyorum.

python3 travel.py -c 35.226.207.165 -u Emre -p 123456

http://35.226.207.165:8080/index.html  üzerinden register

Ardından login istediği gönderdiğimde veya Parisi lokasyonunda hotel aradığımda, gidiş dönüş uçuş aradığımda sorgu responselarını görebildim.

![image](https://user-images.githubusercontent.com/53182424/116428188-5d86de00-a84d-11eb-93a2-eedbfc390fe6.png)

![image](https://user-images.githubusercontent.com/53182424/116428241-6a0b3680-a84d-11eb-94aa-1fea290863df.png)



