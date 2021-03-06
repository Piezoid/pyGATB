# Docker containers

There are three types of docker images that can be built from here:
 * A Debian environment for compiling pyGATB
 * An Alpine build environment
 * Alpine containers for running a standalone Python and Jupyter notebook with pyGATB

## Standalone runtime containers

If your are only interested in trying pyGATB, standalone images can be [found here (24MB)](https://github.com/GATB/pyGATB/releases/download/v0.1a0/docker_images.tar.xz).

### Installation procedure:

```bash
# Load images from package linked above
xz -dc docker_images.tar.xz | docker image load
# Create a container with samples files:
docker create -p 8080:8888 --name pyGATB_notebook_demo pygatb/alpine_notebook
# Start the container:
docker start pyGATB_notebook_demo
# Open the notebook session in the browser
firefox "http://localhost:8080/"
# Stop the instance:
docker stop pyGATB_notebook_demo
```

Instead of using the default volume with samples, you may create a container binding a local directory
containing your notebooks and data:

```bash
docker create -v path_to_work_dir:/home/work -p 8080:8888 --name pyGATB_notebook pygatb/alpine_notebook
# Start it with:
docker start pyGATB_notebook
```

You can also start a python/gatb script with the python interpreter, as follows:
```bash
# Run the sample script:
docker run --rm pygatb/alpine_runtime python3 read_h5.py
# Run a local script:
docker run -v path_to_work_dir:/home/work --rm pygatb/alpine_runtime python3 my_script.py
```


### Building the alpine runtime containers

The `build-alpine-containers.sh` script allows to rebuild the containers from pyGATB source.

### Building manylinux1 wheels

The `build-manylinux.sh` script compile pyGATB in an old Linux distribution (CentOS 5)
against multiple Python3 version. This generates wheels compatible with
many linux distributions and that can be uploaded to PyPI.

## Debian build environment

The `build-debian.sh` build an EGG inside a Debian Jessie container for Python 3.4.

