# JupiterFund Playground

[![Docker Pulls](https://img.shields.io/docker/pulls/jupiterfund/playground.svg)](https://hub.docker.com/r/jupiterfund/playground/)
[![Build Status](https://travis-ci.com/jupiterfund/playground.svg?branch=master)](https://travis-ci.com/jupiterfund/playground)

## Build Image

```bash
# Use existing user id and user name on the host
docker run --rm -d -v /var/run/docker.sock:/var/run/docker.sock jupyter/repo2docker:0.10.0-127.gd9335cf jupyter-repo2docker --user-id 1000 --user-name jovyan --image-name jupiterfund/playground --no-run https://github.com/jupiterfund/playground
```

## How To Use

### Use docker image

```bash
docker run -d -p 8888:8888 jupiterfund/playground jupyter notebook --ip 0.0.0.0 --port 8888
```

### Build & Start

```bash
docker run --rm -d -v /var/run/docker.sock:/var/run/docker.sock jupyter/repo2docker:0.10.0-127.gd9335cf jupyter-repo2docker --user-id 1000 --user-name jovyan --image-name jupiterfund/playground --publish 8888 https://github.com/jupiterfund/playground
```

### More Dependencies

1. The jupyter notebook workspace needs sometimes more dependencies, 
   such as jupiterapis. These dependencies are built and updated automatically 
   on the jupiter server and mounted then into the persisted 
   docker volume `share-lib`.

   ```bash
   # Use existing `share-lib` volume already on the host
   -v share-lib:/share-lib
   ```

2. So as to use the python packages in the workspace, more additional paths 
   must be appended into `PYTHONPATH` by means of:

   * Docker Run Command

     ```bash
     docker run -d -p 8888:8888 -v share-lib:/share-lib jupiterfund/playground bash -c "exec env PYTHONPATH=${PYTHONPATH}:/share-lib jupyter notebook --ip 0.0.0.0 --port 8888"
     ```

   * Python Runtime
 
     ```python
     import sys
     sys.path.append("/share-lib")
     ```
