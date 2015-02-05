# docker-sickbeard

This is a docker container running sickbeard.

It uses supervisord to keep sickbeard running, so if you perform an update on sickbeard, the container won't stop running.

Additionally, below are instructions on how you can transfer an existing sickbeard installation into this container.

## Details

The dockerfile will `git clone` the [current version of sickbeard](https://github.com/midgetspy/Sick-Beard). The installation will be self-container within the conatiner and does not require you to use an external config.ini or db files.

## Building

### Dockerhub

There is also a dockerhub repo ( https://registry.hub.docker.com/u/jfalken/docker-sickbeard/ )

You can obtain this simply via `docker pull jfalken/docker-sickbeard`

### GitHub

1. Clone this repo
2. inside the repo directory, build the image

	`docker build -t <name> .`



## Linking

I use sickbeard via the blackhole method. For this purpose, it's probably best to map a directory on your host as a place to drop the blackholed files. Alternatively you can expose the API port(s) to your host or other containers. These all must be done as a run time parameter when you first run your container creating an image. 

For example:

	`docker run 13ebd -p 8081:8081 -v ~/blackhole:/blackhole`
	
If our build produced an image id of `13ebd`, this command executes the image `13ebd`, maps port `8081` to your localhost and maps the `/blackhole` folder inside the container to `~/blackhole` on your host.

Make sure to adjust your config to the proper blackhole folder (`/blackhole` in this example).

## Copying an existing installation

If you don't want to recreate all your config and files setup, you can copy your existing config details into the running container.

1. First, get the container id of the running container via `docker ps`
2. On your running installation, locate the following files

	`cache.db`, `config.ini` and `sickbeard.db`.
	
3. Copy the files into the container. This is a hacky approach but works perfectly fine: 

	`docker exec -i c479d /bin/bash -c 'cat > /Sick-Beard/cache.db' < cache.db`
	
	`docker exec -i c479d /bin/bash -c 'cat > /Sick-Beard/config.ini' < config.ini`
	
	`docker exec -i c479d /bin/bash -c 'cat > /Sick-Beard/sickbeard.db' < sickbeard.db`
	
	For example, if `c479d` was your container id.
	
4. Go to the sickbeard UI via :8081 and restart sickbeard.
