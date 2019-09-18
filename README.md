[![pipeline status](https://travis-ci.com/FAST-HEP/fast-hep-docker.svg?branch=master)](https://travis-ci.com/FAST-HEP/fast-hep-docker)
[![coverage report](https://codecov.io/gh/FAST-HEP/fast-hep-docker/branch/master/graph/badge.svg)](https://codecov.io/gh/FAST-HEP/fast-hep-docker)
[![Gitter](https://badges.gitter.im/FAST-HEP/community.svg)](https://gitter.im/FAST-HEP/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

fast-hep-docker
=============
A python 3.6 centos7 based docker image equipped to run all the fast-hep software stack.

## Usage

### Docker 

To download the image:
```
docker pull fasthep/fast-hep-docker
```

To execute `fast_carpenter` inside the container:
```
docker run -v $PWD:/work -it fasthep/fast-hep-docker fast_carpenter <datasets.yml> <processing.yml>
```

To run a shell session inside the container:
```
docker run -v $PWD:/work -it fasthep/fast-hep-docker /bin/bash
```

### Singularity 

To download the image:
```
singularity pull docker://fasthep/fast-hep-docker
```

The image also has the ability to access files stored on the UKDC grid via x509 proxy. For the container to access your x509 proxy follow these commands in your current working directory:
```
export X509_VOMS_DIR=/cvmfs/grid.cern.ch/etc/grid-security/vomsdir
export X509_CERT_DIR=/cvmfs/grid.cern.ch/etc/grid-security/certificates
export X509_USER_PROXY=$PWD/.x509_user_proxy
voms-proxy-init -voms <voms> 
```

To run a shell session inside the container:
```
singularity exec -B /cvmfs -B $PWD:/work fasthep-fast-hep-docker.simg /bin/bash
```

## Further Documentation
Is on its way...
