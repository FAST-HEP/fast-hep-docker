[![Docker Build Status](https://img.shields.io/docker/cloud/build/fasthep/fast-hep-docker.svg)](https://hub.docker.com/r/fasthep/fast-hep-docker/builds)
[![Gitter](https://badges.gitter.im/FAST-HEP/community.svg)](https://gitter.im/FAST-HEP/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

fast-hep-docker
=============
A anaconda python 3.7 alpine based docker image equipped to run all the fast-hep software stack.

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
docker run -v $PWD:/work -it fasthep/fast-hep-docker /bin/sh -l
```

### Singularity 
The image has the ability to access files stored on the UKDC grid via x509 proxy. For the container to access your x509 proxy follow these commands in your current working directory:
```
export X509_VOMS_DIR=/cvmfs/grid.cern.ch/etc/grid-security/vomsdir
export X509_CERT_DIR=/cvmfs/grid.cern.ch/etc/grid-security/certificates
export X509_USER_PROXY=$PWD/.x509_user_proxy
voms-proxy-init -voms <voms> 
```

To execute `fast_carpenter` inside the container:
```
singularity run --contain -B /cvmfs -B $PWD:/work docker://fasthep/fast-hep-docker fast_carpenter <datasets.yml> <processing.yml>
```

To run a shell session inside the container:
```
singularity run --contain -B /cvmfs -B $PWD:/work docker://fasthep/fast-hep-docker /bin/sh -l
```

#### CVMFS

The latest fast-hep singularity image is also on CVMFS:
```
/cvmfs/singularity.opensciencegrid.org/fasthep/fast-hep-docker\:version-0.2.0
```


For more on how to use Singularity see https://singularity.lbl.gov/quickstart 

### Shifter

To see a list of images already on an individual NERSC cluster:
```
shifterimg images
```

If `fast-hep-docker` is not in that list or is outdate:
```
shifterimg -v pull docker:fasthep/fast-hep-docker:latest
```

To run a shell session inside the container:
```
shifter --module=cvmfs --image=fasthep/fast-hep-docker:latest sh -l
```


For more on how to use Shifter see https://docs.nersc.gov/programming/shifter/how-to-use/

