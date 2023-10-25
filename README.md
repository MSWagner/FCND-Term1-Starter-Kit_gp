# FCND-Term1-Starter-Kit

Starter kit for FCND Term 1. Python 3 is used for the entirety of term 1.

Packages used:

* [`matplotlib`](https://matplotlib.org/)
* [`jupyter`](http://jupyter.org/)
* [`udacidrone`](https://github.com/udacity/udacidrone). Due to `udacidrone` being updated as the ND progresses, it's recommended for new projects you update udacidrone with `pip install --upgrade udacidrone`.
* [`visdom`](https://github.com/facebookresearch/visdom/)


The recommended way to get up and running:

## [Anaconda Environment](docs/configure_via_anaconda.md)

Get started [here](docs/configure_via_anaconda.md). More info [here](http://conda.pydata.org/docs/).

Supported Sytems: MacOS, Windows, Linux

## Troubleshooting

**NOTE:** If `future` is not installed prior to `pymavlink` this will output an error message similar to the following:

```
  # omitted Traceback
  ModuleNotFoundError: No module named 'future'
  
  ----------------------------------------
  Failed building wheel for pymavlink
  Running setup.py clean for pymavlink
Failed to build pymavlink
Installing collected packages: torchfile, pillow, idna, chardet, urllib3, requests, visdom, future, lxml, pymavlink, utm, websockets, uvloop, udacidrone
  Running setup.py install for pymavlink ... done
  Running setup.py install for udacidrone ... done
Successfully installed chardet-3.0.4 future-0.16.0 idna-2.6 lxml-4.1.1 pillow-5.0.0 pymavlink-2.2.8 requests-2.18.4 torchfile-0.1.0 udacidrone-0.1.0 urllib3-1.22 utm-0.4.2 uvloop-0.9.1 visdom-0.1.7 websockets-4.0.1
```

If the output is `Successfully installed ... pymavlink ..` it's likely the install is fine. You can check by running:

```
python -c "import pymavlink"
```

if the install was successful there should be no output.

**NOTE:** If you're on MacOS you'll need to install developer tools to install `pymavlink`. You can do this by typing the following into a shell:

```
xcode-select --install
```

**NOTE:** On Windows you may need to open a terminal/powershell as an administrator. This can be done by right-clicking the program and selecting "Run as Administrator".

# Docker Conda Environment
I created a dockerfile to create docker image with Ubuntu 18.04 and miniconda 4.3.11 to install and create the necessary conda environment.
- The docker image can be built using following command -
  ```bash
  cd docker/
  docker build -t <my-image-name> .
  ```
- The docker container can be started using following command -
  ```bash
  docker run --network host -it <my-image-name>
  ```
- You can mount the a directory from your pc to the docker container to access the code.
  ```bash
  docker run --network host -it -v /path/on/host:/path/in/container <my-image-name>
  ```
- You can use the `-v` option multiple times to mount multiple paths from your local PC to a single container. Here is an example command to mount two directories:
  ```bash
  docker run --network host -it -v /path/to/local/dir1:/mount/path/dir1 -v /path/to/local/dir2:/mount/path/dir2 my-image-name
  ```
- Once inside the container source the environment
  ```bash
  source activate fcnd_new
  ```
- To run jupyter notebook in the docker container -
  ```bash
  jupyter notebook --ip=0.0.0.0 --port=8888 --allow-root
  ```

## Solutions for MacOS with Apple Silicon (e.g. M1)
I created a dockerfile to create docker image with Ubuntu 18.04 and miniconda 4.3.11 to install and create the necessary conda environment.
- The docker image can be built using following command -
  ```bash
  cd docker/
  docker build --platform linux/x86_64 -t <my-image-name> .
  ```
- The docker container can be started using following command -
  ```bash
  docker run -p 0.0.0.0:8888:8888/tcp --platform linux/x86_64 -it <my-image-name>
  ```
- You can mount the a directory from your pc to the docker container to access the code.
  ```bash
  docker run -p 0.0.0.0:8888:8888/tcp --platform linux/x86_64 -it -v /path/on/host:/path/in/container <my-image-name>
  ```
- You can use the `-v` option multiple times to mount multiple paths from your local PC to a single container. Here is an example command to mount two directories:
  ```bash
  docker run -p 0.0.0.0:8888:8888/tcp --platform linux/x86_64 -it -v /path/to/local/dir1:/mount/path/dir1 -v /path/to/local/dir2:/mount/path/dir2 my-image-name
  ```
- Once inside the container source the environment
  ```bash
  source activate fcnd_new
  ```
- To run jupyter notebook in the docker container -
  ```bash
  jupyter notebook --ip=0.0.0.0 --port=8888 --allow-root
  ```

## Using ipython in docker container to control the drone
- Use the host and add additional ports to the container for controlling the udacidrone simulator
  ```bash
  docker run --add-host=host.docker.internal:host-gateway -p 8888:8888 -p 5760:5760  --platform linux/x86_64 -it -v /path/to/local/dir1:/mount/path/dir1 -v /path/to/local/dir2:/mount/path/dir2 my-image-name
  ```
- Start ipython first and controll the udacidrone simulator with changing the connection argument
  ```python
  from udacidrone import Drone
  from udacidrone.connection import MavlinkConnection
  conn = MavlinkConnection('tcp:host.docker.internal:5760', threaded=True)
  drone = Drone(conn)
  drone.start()
  ```