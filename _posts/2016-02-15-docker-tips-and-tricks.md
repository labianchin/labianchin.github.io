---
layout: post
category : tips
tagline: "Some tips about running and building with docker"
tags : [docker,tips,tools,devops]
---

So you know the Docker basics, but when using it more often some things might miss. As every tool, Docker has its own tips and tricks to be effective.

I have been using Docker for a long time and noticed that some tricks and practices are recurrent. The goal of this is to document tips, tricks and best practices I gathered along my journey of learning and using Docker. Notice that the information here is very dense, so you might come back to look to specific items when needed.

This document is separated in two sections: running (`docker run`) and building (`docker build`/`Dockerfile`).


### Running

Remember every `docker run` creates a new container with the specified image and starts a command inside of it (`CMD` specified in the Dockerfile).
When using this command many times, use the `--rm` flag, so that the container data is removed after it finishes. By default, I tend to use the run command as in the following example:

```sh
docker run --rm -it debian /bin/bash
```

Notice here that `-it` means `--interactive --tty`. It is used to attach the command line to the container after it has started. Those are very useful params when running a container in foreground mode.

#### Check the env

Sometimes it is worth to check what metadata is defined as env vars in a image. Use the `env` command to get this information:

```sh
docker run --rm -it debian env
```

To check the env vars passed to an already created container use:

{% raw %}

```sh
docker inspect --format '{{.Config.Env}}' <container>
```

{% endraw %}

#### Logs

If a container was started in detached mode (`-d`), the logs of it can be retrieved using the following:

```sh
docker logs -f <container_name>
```

Also notice the `-f` tag to follow the upcoming log messages. When you are done, hit `Ctrl-c`.

#### Backup

Data in Docker containers is exposed and shared via volumes flags passed when creating/starting the container. Another good approach is to store data inside Volume Containers. Nevertheless, it is important to backup that data and sometimes it might be necessary to move that data to other host.

Backup using a command like the following:

```sh
docker run --rm -v /tmp:/backup --volumes-from <container-name> busybox tar -cvf /backup/backup.tar <path-to-data>
```

And restore using:

```sh
docker run --rm -v /tmp:/backup --volumes-from <container-name> busybox tar -xvf /backup/backup.tar <path-to-data>
```

This (mine) [StackOverflow answer](http://stackoverflow.com/a/34776997/1046584) has some aliases for these two commands. You can also find them below in the _Aliases_ section.


#### Use docker exec to "enter into a container"

Never install SSH daemon in a container. Use `docker exec` to enter into a container:

```sh
docker exec -it ubuntu_bash bash
```

Use it to inspect and debug running containers. But, ideally, avoid creating automated tooling around it.

Check the documentation of it: https://docs.docker.com/engine/reference/commandline/exec/

#### No space disk left

When running containers and build images several times, disk space may become scarce. When that happens, clear some containers, images and logs.

In order to clean up containers and images, use the following commands:

```sh
docker ps -aq | xargs docker rm # to remove all containers
docker images -aq -f dangling=true | xargs docker rmi # to remove unused images
```

Cleaning up logs is trickier. Still, it can be achieved by running the following command inside the docker host:

{% raw %}

```sh
echo "" > $(docker inspect --format='{{.LogPath}}' <container_name_or_id>)
```

{% endraw %}

- The feature to clear log history was actually rejected: https://github.com/docker/compose/issues/1083
- Consider specifying the `max-size` option to the logging driver on `docker run`: https://docs.docker.com/engine/reference/logging/overview/#json-file-options

#### Aliases

Use these aliases in `.zshrc` or `.bashrc` to clean images and containers, do backup and restore, etc.

{% raw %}

```sh
alias dockercleancontainers="docker ps -aq | xargs docker rm"
alias dockercleanimages="docker images -aq -f dangling=true | xargs docker rmi"
alias dockerclean="dockercleancontainers && dockercleanimages"
alias docker-killall="docker ps -q | xargs docker kill"

# runs docker exec in the latest container
function docker-exec-last {
  docker exec -ti $( docker ps -a -q -l) /bin/bash
}

function docker-get-ip {
  # Usage: docker-get-ip (name or sha)
  [ -n "$1" ] && docker inspect --format "{{ .NetworkSettings.IPAddress }}" $1
}

function docker-get-id {
  # Usage: docker-get-id (friendly-name)
  [ -n "$1" ] && docker inspect --format "{{ .ID }}" "$1"
}

function docker-get-image {
  # Usage: docker-get-image (friendly-name)
  [ -n "$1" ] && docker inspect --format "{{ .Image }}" "$1"
}

function docker-get-state {
  # Usage: docker-get-state (friendly-name)
  [ -n "$1" ] && docker inspect --format "{{ .State.Running }}" "$1"
}

function docker-memory {
  for line in `docker ps | awk '{print $1}' | grep -v CONTAINER`; do docker ps | grep $line | awk '{printf $NF" "}' && echo $(( `cat /sys/fs/cgroup/memory/docker/$line*/memory.usage_in_bytes` / 1024 / 1024 ))MB ; done
}
# keeps the commmand history when running a container
function basher() {
    if [[ $1 = 'run' ]]
    then
        shift
        docker run -e HIST_FILE=/root/.bash_history -v $HOME/.bash_history:/root/.bash_history "$@"
    else
        docker "$@"
    fi
}
# backup files from a docker volume into /tmp/backup.tar.gz
function docker-volume-backup-compressed() {
  docker run --rm -v /tmp:/backup --volumes-from "$1" debian:jessie tar -czvf /backup/backup.tar.gz "${@:2}"
}
# restore files from /tmp/backup.tar.gz into a docker volume
function docker-volume-restore-compressed() {
  docker run --rm -v /tmp:/backup --volumes-from "$1" debian:jessie tar -xzvf /backup/backup.tar.gz "${@:2}"
  echo "Double checking files..."
  docker run --rm -v /tmp:/backup --volumes-from "$1" debian:jessie ls -lh "${@:2}"
}
# backup files from a docker volume into /tmp/backup.tar
function docker-volume-backup() {
  docker run --rm -v /tmp:/backup --volumes-from "$1" busybox tar -cvf /backup/backup.tar "${@:2}"
}
# restore files from /tmp/backup.tar into a docker volume
function docker-volume-restore() {
  docker run --rm -v /tmp:/backup --volumes-from "$1" busybox tar -xvf /backup/backup.tar "${@:2}"
  echo "Double checking files..."
  docker run --rm -v /tmp:/backup --volumes-from "$1" busybox ls -lh "${@:2}"
}
```

{% endraw %}

Sources:

- https://zwischenzugs.wordpress.com/2015/06/14/my-favourite-docker-tip/
- https://website-humblec.rhcloud.com/docker-tips-and-tricks/

### Building Images

In Docker, images are traditionally built using a `Dockerfile`. There is already some nice guides on best practices to build docker images. I recommend taking a look to them:

- https://docs.docker.com/engine/articles/dockerfile_best-practices/
- http://www.projectatomic.io/docs/docker-image-author-guidance/
- http://crosbymichael.com/dockerfile-best-practices.html
- http://crosbymichael.com/dockerfile-best-practices-take-2.html

#### Use a linter

A linter can provide warnings and tips about the Dockerfile. This space is quite new, with some simple solutions but not yet something solid.

There are many options been discussed here: https://stackoverflow.com/questions/28182047/is-there-a-way-to-lint-the-dockerfile

As of January 2016, the most complete seems to be [hadolint](http://hadolint.lukasmartinelli.ch/). Available as online version and terminal version.
It also uses [Shell Check](http://www.shellcheck.net/about.html) to validate the shell commands.

#### Prefer COPY over ADD

`ADD` is very versatile, but generally too magical and hard to understand. `COPY` is a much simpler command to insert files and folder from the build path into the Docker image, and so should be favored. Longer discussion here: https://labs.ctl.io/dockerfile-add-vs-copy/

#### Run a checksum after download and before using the file

Instead of using `ADD` to download and add files to the image. Favor using `curl` and running a checksum after the download.

One nice example to take inspiration is the official [Jenkins Dockerfile](https://github.com/jenkinsci/docker/blob/83ce6f6070f1670563a00d0f61d04edd62b78f4f/Dockerfile#L36):

```
ENV JENKINS_VERSION 1.625.3
ENV JENKINS_SHA 537d910f541c25a23499b222ccd37ca25e074a0c

RUN curl -fL http://mirrors.jenkins-ci.org/war-stable/$JENKINS_VERSION/jenkins.war -o /usr/share/jenkins/jenkins.war \
  && echo "$JENKINS_SHA /usr/share/jenkins/jenkins.war" | sha1sum -c -
```

#### Use a minimal base image

Most of the official images use [`debian`](https://hub.docker.com/_/debian/) as base image. Use this as a default. Remember to also use specific tags, e.g. `debian:jessie`.

If more tools and dependencies are needed, take a look at [`buildpack-deps` images](https://hub.docker.com/_/buildpack-deps/).

If `debian` stills seems too big, consider using [`alpine`](https://hub.docker.com/r/gliderlabs/alpine/) or even [`busybox`](https://hub.docker.com/r/gliderlabs/alpine/). Avoid `alpine` if DNS is needed, there are [some issues](https://github.com/gliderlabs/docker-alpine/blob/master/docs/caveats.md). Also avoid it for languages that use GCC, such as Ruby, Node, Python, etc, this is because `alpine` packs with musl libc which might produce different binaries.

But please, do NOT use [`phusion/baseimage`](https://hub.docker.com/r/phusion/baseimage/). It is an image too big, and many of the things it includes are non essential on Docker containers, [see more here](https://blog.docker.com/2014/06/why-you-dont-need-to-run-sshd-in-docker/).

Other sources:
http://www.iron.io/microcontainers-tiny-portable-containers/

#### Use the layer cache

One nice thing about the `Dockerfile` is the fast rebuilds by using the layer cache. In order to take advantage of this feature, put things that change less often at the top of the `Dockerfile`.

For example, consider installing the code dependencies before adding the code. In NodeJS case:

```
COPY package.json /app/
RUN npm install
COPY . /app
```

More about this: http://bitjudo.com/blog/2014/03/13/building-efficient-dockerfiles-node-dot-js/


#### Clean up in the same layer

When using a package manager to install some software, it is a good practice to clean up the cache generated by the package manager just after the installation. For example:

```
RUN apt-get update && \
    apt-get install -y curl python-pip && \
    pip install requests && \
    apt-get remove -y python-pip curl && \
    rm -rf /var/lib/apt/lists/*
```

Also notice here, that `pip` and `curl` are also removed as they are not needed in for the production application. Remember that the clean up needs to be done in the same `RUN` command, otherwise the data will be persisted in that layer, and removing it later will not have effect in the final image size.

More about this:

http://blog.replicated.com/2016/02/05/refactoring-a-dockerfile-for-image-size/
https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#apt-get

#### Use a wrapper script as ENTRYPOINT, sometimes

A wrapper script can help by taking environment configuration and setting the app configuration. It can even set defaults if environment config is not provided.

Here is one example from [Kelsey Hightower's 12 Fractured Apps](https://medium.com/@kelseyhightower/12-fractured-apps-1080c73d481c#.xn2cylwnk):

```sh
#!/bin/sh
set -e
datadir=${APP_DATADIR:="/var/lib/data"}
host=${APP_HOST:="127.0.0.1"}
port=${APP_PORT:="3306"}
username=${APP_USERNAME:=""}
password=${APP_PASSWORD:=""}
database=${APP_DATABASE:=""}
cat <<EOF > /etc/config.json
{
  "datadir": "${datadir}",
  "host": "${host}",
  "port": "${port}",
  "username": "${username}",
  "password": "${password}",
  "database": "${database}"
}
EOF
mkdir -p ${APP_DATADIR}
exec "/app"
```

Note: **always** use `exec` in shell scripts wrapping the application. In this way, the application becomes PID 1 and it can receive Unix signals.

Also consider using a [dumb init](https://github.com/Yelp/dumb-init) as the base CMD, in this way unix signals can be properly handled. Read more about it here: http://engineeringblog.yelp.com/2016/01/dumb-init-an-init-for-docker.html

#### Log to stdout

Applications inside Docker should log to `stdout`. But some application only log to files. In these cases, a workaround is to symlink that file to stdout.

One example is the official [nginx Dockerfile](https://github.com/nginxinc/docker-nginx/blob/master/Dockerfile):

```
# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log
```

More about it: https://serverfault.com/questions/599103/make-a-docker-application-write-to-stdout

#### Careful with adding data in a volume in Dockerfile

Avoid adding too much data to a folder and then turning it into a `VOLUME`, it will be slow.
Also, still in build time, do not add data to paths that have been previously declared as `VOLUME`, it will not work, data will not be persisted.

Read more about it here in [Jérôme Petazzoni's explanation](https://jpetazzo.github.io/2015/01/19/dockerfile-and-data-in-volumes/).

### Conclusion

That was my list of tips and tricks for Docker. Please, feel free to provide feedback and share your tips and tricks in the comments below.
