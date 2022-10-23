# Rocket.chat and Caddy as reverse proxy using docker engine!

## 0. Requirements:
* own domain associated with the A/AAA record in DNS,
* open port 443 on the server,
* docker and docker-compose.

## 1. Clone repository.
Clone repository to your working directory. I prefer /srv to this. 
```bash
cd /srv
git clone https://github.com/rvva/rocket.chat-caddy 
```
## 1a. [OPTIONAL] Customize docker-compose to suit your requirements
Go to rocket-chat working directory and edit *docker-compose.yml* with your favorite text editor. 
```bash
vim /srv/rocket.chat-caddy/rocket.chat/docker-compose.yml
```
Editing variables is not required - it is optional.

## 2. Run RocketChat
Run docker compose inside rocket.chat working directory:
```bash
cd /srv/rocket.chat-caddy/rocket.chat/
docker-compose up --build -d
```
## 3. Setup Caddy reverse proxy with Let's Encrypt, ZeroSSL or self signed certificate.
Go to caddy working directory and edit Caddyfile.
```bash
vim /srv/rocket.chat-caddy/caddy/Caddyfile
```
Things you need to edit:
* email my@email.com to your valid email,
* mydomain.pl to your domain for rocket.chat,
* reverse_proxy 192.168.0.22:3000 to your local machine address (port 3000 is default for rocket.chat).

Another example:
```bash
{
    # Global options block. Entirely optional, https is on by default
    # Optional email key for lets encrypt
    email bill@red.com
    # Optional staging lets encrypt for testing. Comment out for production.
    # acme_ca https://acme-staging-v02.api.letsencrypt.org/directory
}
chat.red.com {
    reverse_proxy 10.0.44.1:3000
    # If you want to use self signed certifiacate insted Let's Encrypt uncomment section tls inernal
    # tls internal
}
```
If you want to use self signed certifiacate insted Let's Encrypt uncomment section *tls inernal* in Caddyfile.

Run docker compose inside caddy working directory:
```bash
cd /srv/rocket.chat-caddy/caddy/
docker-compose up --build -d
```
## 4. Extras
If you want to change Caddyfile configuration while caddy is running you need to reload configuration. 
You can use for that caddy-reload.sh which is stored in caddy working direcory or command :
```bash
docker exec -it caddy caddy reload --config /etc/caddy/Caddyfile
```
## 5. Logs
Remember logs are your friends. Check them if there are any problems.
```bash
docker logs -f caddy
docker logs -f rockerchat-www
docker logs -f rockerchat-db

```
## 6. Set up in 3 minutes. 
[![Rocket.chat and Caddy in 3 minutes](https://i.ytimg.com/vi/b-snyz6BfRk/hqdefault.jpg)](https://www.youtube.com/watch?v=b-snyz6BfRk&)
