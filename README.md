# postgresql-demo

# Training exercises

* git clone https://github.com/GISTDAsphere/postgresql-demo  // หรือดาวน์โหลด zip มาแตก
* cd postgresql-demo
* docker compose up -d 
* docker compose ps  // หาชื่อ container
* docker exec -it postgresql-demo-db-1 /bin/bash // เข้าไปข้างใน
* apt update
* apt install postgis unzip postgresql-14-postgis-3  // เพื่อให้ได้ shp2pgsql, raster2pgsql, unzip, postgis

* เปิดเว็บไปที่ http://localhost:5050/ (pgAdmin) หรือ http://localhost:8080/ (adminer)
(pgAdmin: admin@gistda.or.th /123456, postgres server = db, user=postgres, password=postgres)
* สร้างฐานข้อมูลชื่อ training
* สั่ง create extension postgis
* สั่ง create extension postgis_raster

# นำเข้า shp files

* cd /data
* unzip 20220115_rice.zip
* cd 20220115_rice
* shp2pgsql -d -I -s 4326 -W UTF8 20220115_rice 20220115_rice > 20220115_rice.sql
* cat 20220115_rice.sql |  psql -d training -U postgres -W -h localhost
* จากนั้นตรวจสอบด้วย pgAdmin หรือ adminer


# นำเข้า DEM

* cd /data/
* raster2pgsql -s 4326 -I -C -M n18_e098_3arc_v2.tif -F -t auto public.srtm > srtm.sql
* cat srtm.sql | psql -h localhost -U postgres -W -d training
* จากนั้นตรวจสอบด้วย pgAdmin หรือ adminer

SELECT st_value(rast,'SRID=4326;POINT(98.48735 18.58933)'::geometry) FROM srtm WHERE ST_Intersects(rast, 'SRID=4326;POINT(98.48735 18.58933)'::geometry);

