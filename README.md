# Docker Bootstrap - my-fork

### Installs

* [Docker](https://www.docker.com/)
* [Docker Compose](https://docs.docker.com/compose/)
* [Traefik Proxy](https://traefik.io/)
* [Watchtower](https://github.com/v2tec/watchtower)

### Instructions

* Copy `.env.dist` to `.env`
* Set all values.
    - _Note:_ Portions of `./init` MUST specify SSH_USER as **root**, so specify values accordingly.  
    - Also, be sure you are working on your local machine when you run `./init`!
* Run `./init`

### Config values

* `SERVER_IP` IP of server
* `SSH_USER` SSH username
* `SSH_PRIVATE_KEY` Location of SSH private key on your local machine
* `LE_EMAIL` Email addressed used for Let's Encrypt certificates generated
    via Traefik

Full blog post on usage
[can be found here](https://jtreminio.com/blog/setting-up-a-static-site-with-hugo-and-push-to-deploy).

# McFateM Additions

Blog posts about my additions to this tool [can be found here](https://static.grinnell.edu/blogs/McFateM).  My fork of this project is [here](https://github.com/McFateM/docker-bootstrap/blob/master/README.md).

Launching a site under the *docker-bootstrap* umbrella involves opening an SSH terminal on the host, in my case that's [ssh://administrator@static.grinnell.edu](ssh://administrator@static.grinnell.edu), setting some key environment variables, and executing a `docker container run` command.  The necessary environment and command snippets include:

## Juan's original snippet

Modified to use `static.grinnell.edu` as the target `HOST`.  This works!

```
NAME=jtreminio_com
HOST=static.grinnell.edu
IMAGE="jtreminio/jtreminio.com"
docker container run -d --name ${NAME} \
    --label traefik.backend=${NAME} \
    --label traefik.docker.network=traefik_webgateway \
    --label traefik.frontend.rule=Host:${HOST} \
    --label traefik.port=80 \
    --label com.centurylinklabs.watchtower.enable=true \
    --network traefik_webgateway \
    --restart always \
    ${IMAGE}
```

## My https://static.grinnell.edu/blogs/McFateM setup

This works!

```
NAME=blogs-mcfatem
HOST=static.grinnell.edu
IMAGE="mcfatem/blogs-mcfatem"
docker container run -d --name ${NAME} \
    --label traefik.backend=${NAME} \
    --label traefik.docker.network=traefik_webgateway \
    --label "traefik.frontend.rule=Host:${HOST};PathPrefixStrip:/blogs/McFateM" \
    --label traefik.port=80 \
    --label com.centurylinklabs.watchtower.enable=true \
    --network traefik_webgateway \
    --restart always \
    ${IMAGE}
```

## My https://mark.mcfate.family/blogs/GrinnellCollege setup

Yep, this works!

```
NAME=blogs-mcfatem
HOST=mark.mcfate.family
IMAGE="mcfatem/blogs-mcfatem"
docker container run -d --name ${NAME} \
    --label traefik.backend=${NAME} \
    --label traefik.docker.network=traefik_webgateway \
    --label "traefik.frontend.rule=Host:${HOST};PathPrefixStrip:/blogs/GrinnellCollege" \
    --label traefik.port=80 \
    --label com.centurylinklabs.watchtower.enable=true \
    --network traefik_webgateway \
    --restart always \
    ${IMAGE}
```
