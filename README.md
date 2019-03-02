# docz installed into Docker container

When you don't want a local installation of yarn, node and rest of required stack for using [docz][1]

[podman][2] is used but you can use docker command in its place with the same result

Podman and docker installed versions are

```bash
$ sudo podman -version
podman version 1.0.0
$ sudo docker -v
Docker version 18.09.2-ce, build 62479626f2
```

Podman's daemon-less architecture will be more flexible and secure than the docker architecture that must be 
running as root. It's a promise at this moment but ... as this is a snippet we will play with this tool although it has not advantages now

# Container creation

Podman looks for docker images in the explicit order showed executing the following command

```bash
$ sudo podman info
host:
...
registries:
  registries:
  - docker.io
  - registry.fedoraproject.org
  - quay.io
  - registry.access.redhat.com
  - registry.centos.org
store:
...
``` 

First looks on the local machine then _docker.io_, _registry.fedoraproject.org_ 
and go on with the registries list.

The tag name **documenting-your-things** is used for container creation executing

```bash
$ sudo podman build -t documenting-your-things ./build
$ sudo podman images
REPOSITORY                          TAG      IMAGE ID       CREATED         SIZE
localhost/documenting-your-things   latest   52ada945bcd7   5 seconds ago   37.3 MB
docker.io/library/alpine            3.9      caf27325b298   4 weeks ago     5.8 MB
docker.io/antora/antora             latest   1493c665c78f   2 months ago    94.2 MB
```

As you can see the image name is preceded by the registry site. The new **documenting-your-things** 
image is registered in the local machine while the base alpine image is registered at docker.io.

Install [docz][1] (intallation files are in local folder) executing

```bash
$ sudo podman run -u $(id -u ${USER}):$(id -g ${USER}) -v $(pwd)/doc:/doc_your_things -it documenting-your-things yarn add docz docz-theme-default --dev
```

## Running development server
 
```bash
$ sudo podman run -u $(id -u ${USER}):$(id -g ${USER}) -e DOCZ_SOURCE=./src -v $(pwd)/doc:/doc_your_things -p 8883:3000 -it  documenting-your-things
Compiled successfully!

You can now view documenting_your_things in the browser.

  Local:            http://localhost:3000/
  On Your Network:  http://10.88.0.85:3000/

Note that the development build is not optimized.
To create a production build, use npm run build.
```

__DOCZ_SOURCE__ environment param defines where source files are.

Obviously, we use **http://localhost:8883/hello** because the instance is running with port 3000 forwarded to **8883**.

---
[1]: https://www.docz.site/introduction/getting-started
[2]: https://podman.io/
