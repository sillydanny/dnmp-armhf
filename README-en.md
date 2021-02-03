Mainly adapted to the DNMP of OneCloud armhf environment

    The Hardware limitations of OneCloud, docker-compose.yml only includes Nginx + MySQL (Mariadb) + PHP7 + Portainer
```
7b659a7425e7   portainer/portainer:latest                 "/portainer"             23 hours ago        Up 20 hours        0.0.0.0:9010->9000/tcp                                                                                                                                                                     portainer
edf0a3fa233e   mydock_nginx                               "nginx -g 'daemon of…"   43 hours ago        Up 20 hours        0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp, 0.0.0.0:8010->8010/tcp, 0.0.0.0:8020->8020/tcp, 0.0.0.0:8030->8030/tcp, 0.0.0.0:8040->8040/tcp, 0.0.0.0:8050->8050/tcp, 0.0.0.0:8080->8080/tcp   nginx
66e5d541db8c   mydock_php                                 "docker-php-entrypoi…"   43 hours ago        Up 20 hours        9000/tcp, 9501/tcp                                                                                                                                                                         php
c67306c6581b   linuxserver/mariadb                        "/init"                  43 hours ago        Up 20 hours        0.0.0.0:3306->3306/tcp
```

---
Docker deploying Nginx MySQL PHP7/PHP5.6/PHP5.4 in one key, support full feature functions.

**[[中文说明]](README.md)**


## 1. Feature
1. Completely open source.
2. Support Multiple PHP version(PHP5.4, PHP5.6, PHP7.0, PHP7.1, PHP7.2, PHP7.3) switch.
3. Support Multiple domains.
4. Support HTTPS and HTTP/2.
5. PHP source located in host.
6. MySQL data directory in host.
7. All conf files located in host.
8. All log files located in host.
9. Built-in PHP extensions install commands.
10. Promise 100% available.
11. Supported any OS with docker.

## 2. Usage
1. Install `git`, `docker` and `docker-compose`;
2. Clone project:
    ```
    $ git clone https://github.com/yeszao/dnmp.git
    ```
3. Add current user to group `docker`：
    ```
    $ sudo gpasswd -a ${USER} docker
    ```
4. Start docker containers:
    ```
    $ cd dnmp
    $ cp env.sample .env
    $ cp docker-compose.sample.yml docker-compose.yml
    $ docker-compose up
    ```
5. Go to your browser and type `http://localhost`, you will see:

![Demo Image](./snapshot.png)

The index file is located at `./www/localhost/index.php`.


## 3.Multiple php version
Default, we create 3 php container, they are PHP7, PHP5.6 and PHP5.4,

We can change easy by modify Nginx configuration `fastcgi_pass`.

For example, [http://localhost](http://localhost) use PHP7, Nginx `fastcgi_pass` is:
```
    fastcgi_pass   php:9000;
```
To use PHP7, change it:
```
    fastcgi_pass   php54:9000;
```
Then reload nginx:
```bash
$ docker exec -it nginx nginx -s reload
```
Done.

## 4.Use composer
We will always use composer in host.

On host, Create a folder for saving composer config file and cache:
```
mkdir ~/dnmp/composer
```
Open ~/.bashrc, add:
```
composer () {
    tty=
    tty -s && tty=--tty
    docker run \
        $tty \
        --interactive \
        --rm \
        --user $(id -u):$(id -g) \
        --volume ~/dnmp/composer:/tmp \
        --volume /etc/passwd:/etc/passwd:ro \
        --volume /etc/group:/etc/group:ro \
        --volume $(pwd):/app \
        composer "$@"
}
```
Make this script affect:
```
source ~/.bashrc
```
Thats all, use composer:
```
cd ~/dnmp/www/
composer create-project yeszao/fastphp project --no-dev
```

## License
MIT
