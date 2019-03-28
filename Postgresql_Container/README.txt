Untuk menjalankan container postgres bisa dengan

1. docker pull postgres
2. Jalankan dengan

docker run -d --name <nama_container> -p <port_host>:5432 -e POSTGRES_PASSWORD=<password_user> postgres

docker run -d --name postgresTes -p 5432:5432 -e POSTGRES_PASSWORD=password postgres

3. Setelah itu bisa diakses dengan 

psql -h localhost -U postgres -d postgres

4. masukan passwordnya yang tadi
