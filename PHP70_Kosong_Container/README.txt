Untuk membuat image dengan
- Nginx
- PHP7.0 & PHP-FPM nya
- Driver php7.0-pgsql

1. download semuanya dan taruh ke 1 folder
2. docker build -t app-php70:nginx .  <--- janganlupa ada titiknya
3. kalau sudah selesai bisa di run imagenya menjadi sebuah container dengan:

docker run -d --name <nama_container> -p <port_host>:80 -p <port_host>:9053 <nama_image>

docker run -d --name odi -p 80:80 -p 9053:9053 app-php70:nginx

4. kalau ingin masuk ke containernya untuk di setting2 lagi bisa dengan:

docker exec -it <nama_container> bash

docker exec -it odi bash
