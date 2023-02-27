# Website-using-Docker-compose
Hosting a simple website using Docker compose method.


### Installation of Docker Composer:

>> Installed composer and provided execute permission.

```sh
[ec2-user@instance1 ~]$ sudo curl -SL https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-linux-x86_64 -o /usr/bin/docker-comp ose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 24.7M  100 24.7M    0     0  5130k      0  0:00:04  0:00:04 --:--:-- 6936k
[ec2-user@instance1 ~]$
[ec2-user@instance1 ~]$ sudo chmod +x /usr/bin/docker-compose
[ec2-user@instance1 ~]$
[ec2-user@instance1 ~]$ docker-compose version
Docker Compose version v2.6.0
[ec2-user@instance1 ~]$
```


>> Created a project directory, then inserted yaml file and uploaded contents from a website template to another folder "webfiles":

```sh
[ec2-user@instance1 ~]$ mkdir web-project
[ec2-user@instance1 ~]$ cd web-project/
[ec2-user@instance1 web-project]$ 
[ec2-user@instance1 web-project]$ ll webfiles/
total 24
-rw-rw-r-- 1 ec2-user ec2-user   510 Jul 30  2019 ABOUT THIS TEMPLATE.txt
drwxrwxr-x 2 ec2-user ec2-user    78 Sep 30  2021 css
drwxrwxr-x 2 ec2-user ec2-user   310 Sep 29  2021 img
-rw-rw-r-- 1 ec2-user ec2-user 15522 Sep 30  2021 index.html
drwxrwxr-x 2 ec2-user ec2-user    91 Sep 29  2021 js
drwxrwxr-x 2 ec2-user ec2-user  4096 Sep 29  2021 webfonts
[ec2-user@instance1 web-project]$
```
</br>

##### Entered the docker-compose.yml and Executed using "docker compose".

```sh
[ec2-user@instance1 web-project]$ docker-compose config
name: web-project
services:
  web:
    container_name: webhost
    image: httpd:alpine
    networks:
      webntw: null
    ports:
    - mode: ingress
      target: 80
      published: "8080"
      protocol: tcp
    restart: always
    volumes:
    - type: bind
      source: /home/ec2-user/web-project/webfiles
      target: /usr/local/apache2/htdocs
      bind:
        create_host_path: true
networks:
  webntw:
    name: web-project_webntw

```

</br>
</br>

Created container from this:

```sh
[ec2-user@instance1 web-project]$ docker-compose up -d
[+] Running 7/7
 ? web Pulled                                                                                                                                   6.0s
   ? 8921db27df28 Pull complete                                                                                                                 0.9s
   ? f2a34d2799ed Pull complete                                                                                                                 0.9s
   ? f404b93686e0 Pull complete                                                                                                                 1.0s
   ? 0fd3f8de080b Pull complete                                                                                                                 2.4s
   ? 2f7563ccd584 Pull complete                                                                                                                 2.9s
   ? 68903caea90d Pull complete                                                                                                                 2.9s 
[+] Running 2/2
 ? Network web-project_webntw  Created                                                                                                          0.0s  
 ? Container webhost           Started                                                                                                          0.7s 
```

</br>

###### Container is running: 

```sh
[ec2-user@instance1 web-project]$ docker container ls -a
CONTAINER ID   IMAGE          COMMAND              CREATED              STATUS              PORTS                                   NAMES
9d9a306fd262   httpd:alpine   "httpd-foreground"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp, :::8080->80/tcp   webhost
[ec2-user@instance1 web-project]$
```

