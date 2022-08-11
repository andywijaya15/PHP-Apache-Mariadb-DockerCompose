![](https://ndik.helloworld.my.id/assets/images/icons8-tree-pastel-96.png) 
# Docker Compose untuk Development PHP

Disini saya membuat docker-compose untuk mempermudah proses development dari 1 pc ke pc lain agar saya tidak perlu mensetup webserver,php dan mariadb secara manual lagi.

## Requirement :
* Linux/Windows(WSL)
* Docker
* Docker Compose

## Penjelasan singkat :

Docker Compose ada untuk mengotomatisasi beberapa container agar kita tidak perlu merunningnya 1 1 karna akan memakan waktu yang lama ketika container yang kita butuhkan berkembang menjadi besar.
Docker Compose yang saya buat akan membuat 2 container dengan nama webserver dan db.
Container Webserver berisi image php7.4 yang sudah saya setup sedemikian rupa agar bisa menjalankan modrewrite untuk kebutuhan konversi url di php.
Di container webserver juga telah saya definisikan volumes untuk mensinkronisasi folder /app ke folder /var/www/html di docker container agar project kita bisa kita ubah di pc kita bisa langsung berubah juga di webserver docker.
Container db berisi image mariadb dengan environment MARIADB_ROOT_PASSWORD untuk mengubah root password mariadb ketika di run.
Di container db juga saya definisikan volumes agar database dari container bisa kita akses melalui host yang saya taruh di /databse jadi saat running container mariadb,kita bisa melihat isi dari folder /var/lib/mysql didalam docker container tersinkron dengan folder /database kita.
Di sini juga saya juga menuliskan port untuk webserver dan db yaitu 81:80 dan 3307:3306 yang artinya saya ingin ketika saya mengakses port 81 / 3307 di perangkat saya maka akan di forward ke port docker dengan port 80 dan 3306.
Saya juga ingin agar webserver saya bisa mengakses mariadb ~~ karena di docker,tiap container terisolasi dan tidak bisa komunikasi 1 dengan yang lain kecuali di network yang sama ~~ . Jadi saya tambahkan links : db di container webserver agar webserver bisa mengakses container db juga.


## Manfaat menggunakan volumes :

Dengan menggunakan volumes,kita bisa menggunakan file di perangkat kita ke container docker tanpa harus masuk ke container dockernya langsung dengan menggunakan bash terminal, sehingga apapun perubahan di sisi aplikasi / database yang di perangkat kita,akan tersinkronisasi juga ke dalam container docker.
Ini bagus untuk development karna kita bisa memasukkan 1 project kita seperti laravel/codeigniter ke dalam folder /app, kemudian melakukan CRUD database di mariadb,data-data tersebut akan berada di flashdisk kita.Sehingga program dan database kita tidak akan berbeda ketika dijalankan ke PC/Laptop yang lain

## Cara penggunaan:

* Pastikan sudah terinstall docker dan docker compose di perangkat yang digunakan,disini saya menggunakan :
	* Manjaro Linux
	* Windows dengan WSL Debian
	* Docker
	* Docker Compose V2.9.0

Untuk installasi WSL di Windows  bisa dibaca [disini](https://docs.microsoft.com/en-us/windows/wsl/install) 
Untuk installasi Docker Compose bisa dibaca [disini](https://docs.docker.com/compose/install/compose-plugin/) 
	
Cara mengeceknya dengan cara docker version untuk mengecek versi docker dan docker compose version untuk mengecek versi docker compose

* masukkan folder codeigniter/laravel ke folder app
* masuk ke directory yang ada docker-compose.yml kemudian buka terminal dan ketikkan :
>		docker compose up -d

Tunggu hingga proses selesai,jika tidak ada error maka php,apache dan mariadb sukses berjalan di perangkat kalian.
Untuk mariadb silahkan tunggu sampai mariadb active dengan sempurna,karena biasanya mariadb memerlukan waktu untuk running dengan sempurna.
Coba akses localhost:81 atau masuk ke database client dan akses 127.0.0.1:3307 maka anda sudah bisa mendevelop aplikasi anda dengan portable.
Untuk mematikan docker compose silahkan ketikkan :
>		docker compose down

Maka sudah webserver dan db sudah tershutdown dengan sempurna