#### Docker

#### Postgres 
1. DB konteynerinin backup'ı aşağıdaki komut ile alınır. 

       docker exec -t “container id” pg_dumpall -c -U “Db user” > dump_`date +%d-%m-%Y"_"%H_%M_%S`.sql

2. DB konteynerinin backup'ı aşağıdaki komut ile de alınabilir.

       docker exec -i <container_id> pg_dump "user=db_username host=db_hostname dbname=dbname password=db_pasword" >`date +"%m-%d-%y"`.sql

3. Alınan backup'tan DB'yi aşağıdaki komut ile restore edebiliriz.

       cat your_dump.sql | docker exec -i your-db-container psql -U "your-db-user" -d "your-db-name"

#### Mongo
1.  DB konteynerinin backup'ı aşağıdaki komut ile alınır.

       docker exec -i "container id" mongodump --host=$HOSTNAME --authenticationDatabase=$DATABASE --username=$USERNAME --password=$PASSWORD --out /dump/$DUMP_PATH-`date +"%m-%d-%y"

2. Server içerisinde restore edeceğimiz backup folder'ı;
   
   - Aşağıdaki komut ile konteyner içine kopyalanır.  

         docker cp <example-path> <container_id>:/dump/

   - DB konteyneri içerisine girilir ve aşağıdaki komut ile restore edilir.

         mongorestore --username <db_username> --password <db_password> -d <db_name> /dump/<folder_name>/


#### Kubernetes

#### Postgres 
1. DB konteynerinin backup'ı default namespacede aşağıdaki komut ile alınır. 

       kubectl exec -i “pod id” -- pg_dump "user=$USERNAME host=$HOSTNAME dbname=$DATABASE password=$PASSWORD" > -dump_`date +%d-%m-%Y"_"%H_%M_%S`.sql

2. Alınan backup'tan DB'yi aşağıdaki komut ile restore edebiliriz.

       cat your_dump.sql | kubectl exec -i “pod id” -- psql -U "your-db-user" -d "your-db-name"


#### Mongo
1. DB konteynerinin backup'ı default namespacede aşağıdaki komut ile alınır. (mongo)

       kubectl exec -i "“pod id" -- mongodump --username=$USERNAME --password=$PASSWORD --authenticationDatabase=$DATABASE --out /dump/$DUMP_PATH-`date +"%m-%d-%y"`

2. Server içerisinde restore edeceğimiz backup folder'ı;
   
   - Aşağıdaki komut ile konteyner içine kopyalanır.  

         kubectl cp <example-path> <pod_id>:/dump/

   - DB konteyneri içerisine girilir ve aşağıdaki komut ile restore edilir.

         mongorestore --username <db_username> --password <db_password> -d <db_name> /dump/<folder_name>/