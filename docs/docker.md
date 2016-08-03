---
title: "Docker"
slug: "docker.md"
weight: 10
---

This page describe a way to build a docker image for the UCF fork
of VISH.

:warning:  DO NOT USE THIS IN PRODUCTION :warning:

This image is built with the development configuration and should be
used only testing purposes.

The steps to build the docker image are the following.

```bash
# Clone the UCF VISH docker repository
git clone git@git.rnd.alterway.fr:UCF/vish_docker.git
cd vish_docker

# Create a ssh key that has access to project repository git@git.rnd.alterway.fr:UCF/vish.git
ssh-kkeygen -t rsa

# Build docker image
docker build -t vish:ucf  ./

# Start container
docker run -d -p 3000:3000 vish:ucf
```

The server boot takes around 30 seconds. After the boot you can access
your UCF MOOC in any browser at the URL http://localhost:3000.

Default login is **demo@vishub.org** and password is **demonstration** .

