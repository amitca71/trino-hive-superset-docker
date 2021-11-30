# Cloud-native Lakehouse:  
## kafka + spark + Deltalake + (Trino (prestosql) + Hive + Minio)/dremio + Superset. 
### Technologies:
#### apache kafka
#### apache spark
#### apache deltalake
#### Query Engine is `Trino (PrestoSQL)`\Dremio
#### Metadata Store is `Apache Hive`
#### Object Storage is `Minio (S3 compatable)`
#### Data Viz is `Apache Superset`
![image](https://user-images.githubusercontent.com/5821916/144006020-8bfc96ca-5475-4be1-a7db-355622ad0404.png)


## Get things running
1. Clone repo
2. Install docker + docker-compose
3. Run `docker-compose --compatibility up`
4. Run `bash boot.sh`
5. Checkout the service endpoints:

Trino: `http://localhost:8080/ui/` (username can be anything) <br>
Dremio: `http://localhost:9047`
Minio: `http://localhost:9001/` (username: `minio_access_key`, password: `minio_secret_key`)<br>
Superset: `http://localhost:8088/` (username: `admin`, password: `admin`)<br>
kafka: `http://localhost:9021/ ` <br>
spark: `http://localhost:9999 ` (get key: docker-compose exec pyspark bash -c "jupyter server list")
## Kafka connect - stream data from mysql to minio: 
1. create S3 bucket: minio-sink-bucket (minio) 
2. create kafka connectors:
##### Start S3 minio connector
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @s3-minio-sink.json
##### Start MySQL connector
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @source.json 
##### insert/update records on mysql (otional- few records already exist in mysql table and would stream as server start)  
   - docker-compose exec mysql bash -c 'mysql -u $MYSQL_USER  -p$MYSQL_PASSWORD inventory -e "select * from customers"' 
   - docker-compose  exec mysql bash -c 'mysql -u $MYSQL_USER  -p$MYSQL_PASSWORD inventory' 
      mysql> insert into customers values(default, 'John', 'Doe', 'john.doe@example.com'); 
      mysql> update customers set first_name='Jane', last_name='changed' where last_name='Thomas'; 
## Spark - transform data to deltalake format:
1. log into spark 
2. execute notebook
## Hive - create delta hive tables:
1. docker-compose exec hive hive
2. docker-compose exec hive hive -e 'create database amithive;'
3. docker-compose exec hive hive -e "$(cat hive_command.hql)"
## Connect to Trino in Superset:
1. Go to `data` dropdown and click `databases`
2. Click the `+ database` button
3. For `Select a database to connect` choose `trino`
4. In `SQLALCHEMY URI` put `trino://hive@trino-coordinator:8080/hive` 
## Drimio  (works only with real S3- for me
1. http://localhost:9047/ 
2. create user and pass
3. connect to S3 bucket
4. set type as deltalake 
## Connect to Drinio in Superset (Drimio doesnt require hive, but add dificulties to work with minio. work only with real S3:
1. Go to `data` dropdown and click `databases`
2. Click the `+ database` button
3. For `Select a database to connect` choose `Drimio`
4. In `SQLALCHEMY URI` put `dremio://user:pwd@host:31010/` 
### sqllab
1. choose: `trino...


