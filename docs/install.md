# Installation

### Options
* [Mac](install.md#mac)
* [Windows](install.md#windows)
* [Linux](https://eulertour.com/docs)
* [Anaconda](installation.md#anaconda)
* [Python `virtualenv`](install.md#python-virtualenv)
* [Docker](install.md#docker)

## Mac
You will need to install [cairo](https://cairographics.org), [sox](https://sourceforge.net/projects/sox/), and [ffmpeg](https://www.ffmpeg.org/). If you would like to use [LaTeX](https://www.latex-project.org/), you can install a LaTeX distribution (such as MacTeX). You will also need [**Python 3.7**](https://www.python.org/downloads/release/python-377/) (3.8 won't work).

You can install these from the linked websites, or you can install them all with [Homebrew](https://brew.sh).
```sh
brew install cairo sox ffmpeg mactex
```
You also need `pkg-config` from brew:
```sh
brew install pkg-config
```

Then install manim from PyPI:
```sh
pip install manimlib
```
If you encounter any errors, check the [issues tab](https://github.com/3b1b/manim/issues).

## Windows

You will need ffmpeg, cairo, and sox. If you would like to use LaTeX, you can download a LaTeX distribution (such as [MiKTeX](https://miktex.org/download)).

 1. Installing ffmpeg is harder on Windows devices. You should follow [these instructions](https://www.wikihow.com/Install-FFmpeg-on-Windows) to get it installed properly.

 2. You can install cairo from [here](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pycairo). For most users, `pycairo‑1.19.1‑cp37‑cp37m‑win32.whl` will do fine. Then, install it with pip:

```sh
pip3 install C:/path/to/pycairo‑1.19.1‑cp37‑cp37m‑win32.whl
```

 3. Then, install [sox](https://sourceforge.net/projects/sox/).

 4. A LaTeX distribution is optional ([MiKTeX](https://miktex.org/download) is recommended).

 5. Clone this repository and install from pip.

Clone the repository:
```sh
git clone https://github.com/3b1b/manim.git
cd manim
```
Open `requirements.txt` and change the pycairo version to whichever you downloaded (likely `1.19.1`).
```sh
# Open it from the command line
requirements.txt
```
Then, install from the requirements file:
```sh
pip install -r requirements.txt
```
Check the [issues tab](https://github.com/3b1b/manim/issues) if there are any errors.

## Anaconda

 * Install sox and (optionally) LaTeX for your operating system.
  - [Mac](#mac)
  - [Windows](#windows)
 * Clone the repository, and then create a virtual environment:
```sh
git clone https://github.com/3b1b/manim.github
cd manim
conda env create -f environment.yml
```
 * **WINDOWS ONLY** Install `pyreadline` via `pip install pyreadline`.

## Python `virtualenv`

 * Install `virtualenv` and `virtualenvwrapper`.
 * Clone and then create the environment:
```sh
git clone https://github.com/3b1b/manim.git
cd manim
mkvirtualenv -a manim -r requirements.txt manim
```

## Docker

Since it's a bit tricky to get all the dependencies set up just right, there is a Dockerfile and Compose file provided in this repo as well as [a premade image on Docker Hub](https://hub.docker.com/r/eulertour/manim/tags/). The Dockerfile contains instructions on how to build a manim image, while the Compose file contains instructions on how to run the image.

The prebuilt container image has the manim repository included.

`INPUT_PATH` is where the container looks for scene files. You must set the `INPUT_PATH`
environment variable to the absolute path containing your scene file and the
`OUTPUT_PATH` environment variable to the directory where you want media to be written.

1.  [Install Docker](https://docs.docker.com)
2.  [Install Docker Compose](https://docs.docker.com/compose/install/)

3.  Render an animation:
```sh
INPUT_PATH=/path/to/dir/containing/source/code \
OUTPUT_PATH=/path/to/output/ \
docker-compose run manim file.py SceneName -flags
```

The command needs to be run as root if your username is not in the docker group.
`file.py` can be any relative path from your `INPUT_PATH`

![docker diagram](https://github.com/3b1b/manim/raw/master/manim_docker_diagram.png)

After running the output will say files ready at `/tmp/output/`, which refers to path inside the container. Your `OUTPUT_PATH` is bind mounted to this `/tmp/output` so any changes made by the container to `/tmp/output` will be mirrored on your `OUTPUT_PATH`. `/media/` will be created in `OUTPUT_PATH`.

**`-p` won't work** with this installation as manim would look for a video player in the container system, which it does not have.

The first time you execute the above command, Docker will pull the image from Docker Hub and cache it. Any subsequent runs until the image is evicted will use the cached image. Note that the image doesn't have any development tools installed and can't preview animations. Its purpose is building and testing only.
