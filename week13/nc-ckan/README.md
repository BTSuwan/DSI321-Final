<h1>Build Command:</h1>

```
    git clone https://gitlab.com/charon1/nc-ckan.git
    cd nc-ckan
    docker-compose up -d
```

<h1>สำหรับการติดตั้งบน cloud</h1>

<h2>แก้ไขไฟล์ .env ใน โปรเจค</h2>


```
DEFAULT_URL=http://localhost            // url สำหรับเข้า ckan  หรือ ถ้าเป็นบน server ใช่เป็น ip เช่น http://52.123.51.3 หรือ http://domain_name.com เป็นต้น
CKAN_SYSADMIN_NAME=ckan_admin           // username admin ของ ckan 
CKAN_SYSADMIN_PASSWORD=ckan_admin       // password admin ของ ckan 

```
<h2>แก้ไขไฟล์ docker-compose.yml ใน โปรเจค</h2>

 ```
บรรทัดที่ 33 เป็นการตั้งค่า default สำหรับ database
db: 
    container_name: db-${PROJECT_NUMBER}
    # image: mdillon/postgis:11
    image: kran13200/db:1.0.0
    environment:
      - DATASTORE_READONLY_PASSWORD=${DATASTORE_READONLY_PASSWORD} // passowrd ของ datastore
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}                      // passowrd ของ database postgres
      - POSTGRES_USER=ckan                                          // username ของ database postgres
      - POSTGRES_DB=ckan                                            // database nane ของ database postgres
      - PGDATA=/var/lib/postgresql/data/some_name/
      - TZ=${TZ}                                                     // Time Zone ของ database postgres
    networks:
      - ckan-networks
    volumes:
      - pg_data:/var/lib/postgresql/data

บรรทัดที่ 65 เป็นการตั้งค่า default สำหรับ pgadmin
pgadmin:
    container_name: pgadmin_container
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: pgadmin                                // username ของ pgadmin
      PGADMIN_DEFAULT_PASSWORD: pgadmin                             // passowrd ของ pgadmin
      # PGADMIN_CONFIG_SERVER_MODE: 'False'
    volumes:
       - pgadmin:/root/.pgadmin
    ports:
      - "0.0.0.0:8080:80"
    networks:
      - ckan-networks
    restart: unless-stopped
    
ในส่วน ของ port 
จะมี 
    ports: อยู่ในทุกๆ service โดย ค่าจะ
            เป็น port ภายนอก หรือ port ที่เราจะสามารถเข้าถึงได้
              |
              V
    0.0.0.0:8080:80 <--  เป็น port ภายใน ซึ่งจะเป็น port สำหรับคุยกัน ภายใน docker networks
```
