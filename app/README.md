## App
ini adalah directory tempat penyimpanan framework project untuk disinkronkan ke container webserver di docker container.

==Note untuk Laravel==
Silahkan ganti file docker-compose.yml bagian container webserver dan ganti volumes :
> ${PWD}/app:/var/www/html

menjadi :

> ${PWD}/app/public:/var/www/html