# Deploy podman container images on heroku

I made the switch from docker to [podman](https://podman.io), while switching from ubuntu 19.10 to fedora 32. The main reason are cgroups v2 (Dan Walsh wrote about it [here](https://www.redhat.com/sysadmin/fedora-31-control-group-v2)).

_**Disclaimer:** I was using `podman version 1.9.3` and not the recently released `v2`. Everything should work the same, but I didn't try it._

I recently encountered a problem while trying to deploy a small python application to [heroku](https://heroku.com/what). The heroku-cli provides the [`heroku container`](https://devcenter.heroku.com/articles/heroku-cli-subcommands#heroku-container) command to conveniently manage your applications container.
But because this command is build with "container" being equivalent to "docker container" I ran into two problems:
1. The heroku cli complained that it couldn't find `docker` on my machine
2. It also rejected my podman images, because of a wrong image manifest

## Wanted dead or alive: docker

Error message:
```bash
$ heroku container:push worker                                  
=== Building worker (/path/to/my/project/Dockerfile)
▸    Error: docker build exited with Error: Cannot find docker, please ensure docker is installed.
▸    If you need help installing docker, visit https://docs.docker.com/install/#supported-platforms
```

The problem is obviously that the heroku-cli can't find `docker`. This is quite easy to resolve, because the podman cli is docker-compatible. We just need to create a symlink from `podman` to `docker`:
```bash
$ sudo ln -s $(which podman) /usr/local/bin/docker
```

Running the command again will pass the previous and show us the next error.

Problem 1 solved ✅

---

_I didn't verify, but you should be able to achieve the same by installing `podman-docker`, which additionally creates links between all Docker CLI man pages and podman._

## Wrong manifest version

Error massage:
```bash
$ heroku container:push worker
STEP 1: FROM python:3.8-slim
# ...trimmed...
=== Pushing worker (/path/to/my/project/Dockerfile)
Getting image source signatures
Copying blob a8c3d457206d done
# ...trimmed...
Writing manifest to image destination
Error: Error copying image to the remote destination: Error writing manifest: Error uploading manifest latest to registry.heroku.com/my-project/worker: unsupported
▸    Error: docker push exited with Error: 125
```

This error seems to be about some kind of unsupported image manifest. What's this manifest? – The manifest is a file, which "provides a configuration and set of layers for a single container image for a specific architecture and operating system" [⁽¹⁾](#citations). There are multiple formats, like the [`v2s2` (Docker Image Manifest Version 2, Schema 2)](https://docs.docker.com/registry/spec/manifest-v2-2/), or [`oci` (OCI Image Manifest)](https://github.com/opencontainers/image-spec/blob/master/manifest.md).

After a bit of reading around I found the issue [podman/libpod#1719](https://github.com/containers/libpod/issues/1719#issuecomment-433648221) about problems of amazon webservices with the podman manifest.

The solution is to specify the manifest format to be `v2s2` on `podman push`, using the [`--format`](https://www.mankier.com/1/podman-push#--format) option. This is oddly only possible if you push to a directory, so we will need to run:
```bash
$ mkdir $sometmpdir
$ podman push --format=v2s2 registry.heroku.com/my_image/worker dir:$sometmpdir
```

Since we don't want to have the image in a local temp-directory, but on heroku we need to copy it over using [`skopeo`](https://github.com/containers/skopeo):
```bash
$ skopeo --debug copy dir:$sometmpdir docker://registry.heroku.com/my_image/worker:latest
```

Now it is possible to run our image with:
```bash
$ heroku container:release worker
```

Problem 2 solved ✅

## Summary
We finally have our podman container running seamlessly on heroku.

The problem of people building container tools and integrations just for docker container is annoying, but overcomeable, mainly thanks to the podmans docker-compatable cli.

To make this workaround easier, I would like the podman cli to enable the `--format` option also for normal pushes, which doesn't seem to complicated imho.

But actually this issue should be addressed by heroku, by properly supporting OCI-Images.

## Citations
1. Opencontainers. (2019, February 05). Opencontainers/image-spec/manifest.md. Retrieved June 25, 2020, from https://github.com/opencontainers/image-spec/blob/master/manifest.md