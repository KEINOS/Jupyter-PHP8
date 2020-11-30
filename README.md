<!-- markdownlint-disable MD033 -->

# Dockerfile of Jupyter with PHP8 Kernel

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)

## Introduction

This is a sample Dockerfile to run [*Jupyter*](http://jupyter.org) with PHP8.0 (JIT enabled) kernel using patched **[Jupyter-PHP](https://github.com/Litipk/Jupyter-PHP)**.

- Works on: ARM v6l,v7l,ARM64 (Raspbian, RaspberryPi) and Intel/AMD (64bit Windows10 and macOS) architectures.
- For more detals about Jupyter-PHP see: [Jupyter-PHP](https://github.com/Litipk/Jupyter-PHP) @ GitHub
- For NON Docker Users
  - To just fix the [Jupyter-PHP-Installer](https://github.com/Litipk/Jupyter-PHP-Installer), use the below patch.
    - [diff.jupyter-php.patch](https://github.com/KEINOS/Jupyter-PHP8/blob/docker/diff.jupyter-php.patch)

## Usage

### Build image

```bash
docker build -t jupyter:local .
```

### Run container

```bash
docker run -p 8001:8000 jupyter:local

# To shutdown the container and jypyter server press Ctrl+C
```

> **NOTE**
>
> <sup>Don't forget to publish the port as `-p 8001:8000` as above. This will port-forward the 8001 port of your `localhost` to the 8000 port of the container in the Docker network.</sup>
>
> <sup>If the 8001 port is already in use in you local, then change it to any unused port number.</sup>
>
> <sup>By default, the Jupyter server in the container starts with token authentication enabled.</sup>
>
> <sup>After the web server launches, a token will be displayed. Copy it and access the Jupyter lab container from your browser with the provided token. (See the sample usage section)</sup>

### Share/mount local data directory

If you want the notebooks saved in your local or mount files from local to the container, then you need to mount the target directory as a volume to `/workspace` of the container.

The below sample mounts the local `data` directory to the container as `/workspace`.

```bash
docker run --rm -p 8001:8000 -v $(pwd)/data:/workspace jupyter:local
```

#### Direct TTY access to the container

If you want to interact locally via TTY then specify the shell as below. This will over-ride the boot process of Jupyter lab so you need to boot it manually.

```bash
docker run -it jupyter:local /bin/sh
```

- Note that you can access/browse the files/directories in the container from Jupyter's terminal/console as well.

## Sample usage

```shellsession
$ git clone https://github.com/KEINOS/Jupyter-PHP8.git && cd Jupyter-PHP8
...

$ docker build -t jupyter:local .
...(this takes time)...

$ docker run --rm -it -p 8001:8000 -v $(pwd)/data:/workspace jupyter:local
[I 09:19:03.520 LabApp] Writing notebook server cookie secret to /root/.local/share/jupyter/runtime/notebook_cookie_secret
[I 09:19:04.470 LabApp] JupyterLab extension loaded from /usr/local/lib/python3.9/site-packages/jupyterlab
[I 09:19:04.470 LabApp] JupyterLab application directory is /usr/local/share/jupyter/lab
[I 09:19:04.475 LabApp] Serving notebooks from local directory: /workspace
[I 09:19:04.475 LabApp] Jupyter Notebook 6.1.4 is running at:
[I 09:19:04.475 LabApp] http://a9b6b6cde2b3:8000/?token=2c7a9b0dce099eb8c2571072e51b0bce499143dab0aff271
[I 09:19:04.476 LabApp]  or http://127.0.0.1:8000/?token=2c7a9b0dce099eb8c2571072e51b0bce499143dab0aff271
[I 09:19:04.476 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 09:19:04.488 LabApp]

    To access the notebook, open this file in a browser:
        file:///root/.local/share/jupyter/runtime/nbserver-1-open.html
    Or copy and paste one of these URLs:
        http://a9b6b6cde2b3:8000/?token=2c7a9b0dce099eb8c2571072e51b0bce499143dab0aff271
     or http://127.0.0.1:8000/?token=2c7a9b0dce099eb8c2571072e51b0bce499143dab0aff271

# Then launch a browser and access to the container. In the case above the URL to access will be:
#   http://localhost:8001/?token=2c7a9b0dce099eb8c2571072e51b0bce499143dab0aff271
```

## Info

- Repo: https://github.com/KEINOS/Jupyter-PHP8
  - Branch: `docker` (Orphan branch)
  - Fork of: https://github.com/Litipk/Jupyter-PHP
- Base Image: [KEINOS/PHP8-JIT](https://github.com/KEINOS/Dockerfile_of_PHP8-JIT)
- Image OS: Alpine Linux
- User: `root`
- Default port number of Jupyter lab to listen: 8000
- List of Jupyter services installed:
  - jupyter core
  - jupyter-notebook
  - qtconsole
  - ipython
  - ipykernel
  - jupyter client
  - jupyter lab
  - nbconvert
  - pywidgets
  - nbformat
  - traitlets
- Architecture (CPU) Tested:
  - Intel/AMD: Windows10 (Docker v20.10.0-rc1), macOS Catalina (Docker v19.03.13)
  - ARM v7l: Raspbian Jessie (Docker v19.03.13)

## Troubleshoot

If you get "`Clearing invalid/expired login cookie username-localhost-XXXX`" error while booting the container, then check if someone isn't accessing the container (opening the browser). If so, then close it and try again. The user is trying to access with the old token.

## How to contribute

Any PR is welcome but please follow the basic rules below.

- **PR to the `docker` branch** and no other branch.
- Before PR, rebase/merge the changes made in the upstream and see if it still works.
- Consider to [Draft PR](https://github.blog/2019-02-14-introducing-draft-pull-requests/) before get going what you are willing to do, so the other people know about it. Also, don't hesitate to drop down your draft PR if you get tired. Take your time.
- You can [issue](https://github.com/KEINOS/Jupyter-PHP8/issues) in Japanese and Spanish as well.

## License

[Jupyter-PHP](https://github.com/Litipk/Jupyter-PHP) and [this repo](https://github.com/KEINOS/Jupyter-PHP8) are licensed under the [MIT License](https://github.com/KEINOS/Jupyter-PHP8/blob/master/LICENSE) by [Litipk](https://github.com/Litipk), [his contributors](https://github.com/Litipk/Jupyter-PHP/graphs/contributors), [KEINOS](https://github.com/KEINOS) and [his contributors](https://github.com/KEINOS/Jupyter-PHP8/graphs/contributors).
