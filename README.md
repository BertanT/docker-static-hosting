# docker-static-hosting
Cute little setup for hardening safety when hosting potentially vulnerable static websites using Docker. Implements read-only bind mounts to isolate the server from site source.

* I use setups very similar to this for hosting my personal websites based on WatchDuck, iWeb, etc. Just wanted to release it as a template for self reference and sharing. Please customize to your own needs before using!
* The Directory structure should look something like below. Do not rename any files other than the parent directory.
```
YourSite/
├─ nginx-server.conf
├─ docker-compose.yml
├─ StaticContent/
```
* Files in `StaticContent` contain the static content you would like to host. They get handed over to Nginx with a read only bind mount.
* You should be able to start hosting everything with `docker compose up`. If in rootless mode (ideal), make sure you [enable lingering for your own user](https://docs.docker.com/engine/security/rootless/tips/).
* By default, Nginx in the container listens on port 80. Docker then binds the container port 80 to the host localhost port 8080. You can configure this in docker compose like so:
```
# HOST_IP:HOST_PORT:CONTAINER_PORT
ports:
  - 127.0.0.1:8080:80
```
* ***Warning!!!*** Security header inheritance via `add_header_inherit merge` is dependent on nginx version 1.29 and higher. The settings present here in `nginx-server.conf` when used with other versions may remove your security headers silently!
* The Nginx config here is just an example. There really isn't one-size-fits-all Nginx config! Make sure to read the comments, look at Nginx documentation, and write one tailored to your specific needs. If using/customizing the one here, pay extra attention to the caching rules.
* Please make sure the `resources:` block in `docker-compose.yml` allocates enough resources for your site!
* There is a lot more nuance to a complete setup! Again, please refer to to comments within the files here as well as Docker and Nginx documentation.
* I hope this is useful :)
