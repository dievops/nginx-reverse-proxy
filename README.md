# Simple steps for a simple nginx reverse proxy.  

1- Install docker and docker-compose.  
    - https://get.docker.com. 
    - https://docs.docker.com/compose/install/. 

2- Create a file with .conf extension like awesome-reverse.conf:  

server  {
listen 0.0.0.0:80 default_server;
error_log  /dev/stderr debug;
access_log  /dev/stdout main;
rewrite_log on;
client_header_timeout  30;
client_body_timeout  30;
charset  UTF-8;

location / {
proxy_pass https://proxy-my-request-to; #this is where your requests will arrive
}

location /status {
return 200 'im working dude';
}
}

3- Create a docker-compose.yaml file.  

services:
  reverse:
    container_name: reverse
    hostname: reverse
    restart: always
    image: nginx
    ports:
      - 80:80
    volumes:
      - $PWD/awesome-reverse.conf:/etc/nginx/conf.d/awesome-reverse.conf #Here you should map your file created on step number 3 with nginx using a volume statement.


### YOUR SUPER REVERSE PROXY. 

4- Start your reverse proxy and check error:  

docker-compose up

5- Once you check errors and everything is going fine, start it detached and forever with:

docker-compose up -d. 

6- logs with.  

docker-compose logs -f. 

### Sample folder structure:  

reverse-nginx-proxy/
├── awesome-reverse.conf
└── docker-compose.yaml
