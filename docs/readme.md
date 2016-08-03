---
title: "UCF MOOC"
type: "index"
weight: 0
---

UCF MOOC is a platform for creation and exploration of MOOC courses. It is
a fork of the open source project called [VISH](https://github.com/ging/vish).

A special export have been developed called **SCORM UCF**. It brings a
player that can communicate with other applications developed by the
partners of UCF project. More details about SCORM UCF can be found at
[Player UCF project](https://git.rnd.alterway.fr/UCF/mooc_player/).

An easy way to try UCF MOOC is to run it in docker. The details about
the docker version are described in [docker.md](docker.md).

The [installation.md](installation.md) document explains the steps to
install UCF MOOC for development.

For common problems found during installation first check out the
[troubleshooting.md](troubleshooting.md) section.

## Configuring UCF

Add to the file `config/application_config.yml` under the desired
environment the section **ucf**, with **enable: true**. For example:

```
# Example of application_config.yml with UCF enabled

development:
  name: "UCF MOOC development"
  domain: "localhost:3000"
  test_domain: true
  # ... any other attributes
  ucf:
    enable: true

```

## Screenshots

Homepage

![Screenshot of UCF MOOC Homepage](img/ucf-home.png)

MOOC course editor

![Screenshot of UCF MOOC course editor](img/ucf-editor.png)

UCF special export inside MOOC course editor

![Screenshot of UCF special export inside MOOC course editor](img/ucf-export.png)

UCF settings inside MOOC course editor

![Screenshot of UCF settings inside MOOC course editor](img/ucf-settings.png)
