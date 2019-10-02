#Base Image
FROM python:3.7.4-buster
LABEL maintainer="andrew.naylor@sheffield.ac.uk"

USER root

#re-order the lines of code into the fewest stages but once it's all been built properly

# Install Debian Packages
RUN apt-get update && \
    apt-get install -y cmake && \
    apt-get clean

# #Install XRootD + pxyrootd 
# RUN apk add --no-cache libpng libpng-dev freetype freetype-dev llvm llvm-dev 
# RUN apk add --no-cache make cmake git curl openssl
#apk add openssl-dev readline-dev curl-dev libxml2-dev krb5-dev json-c-dev linux-headers

# openssl-dev
# readline-dev
# curl-dev
# libxml2-dev
# krb5-dev
# json-c-dev

# linux-headers

# fuse-dev (?)

# alpine-sdk

#Install pip packages
RUN pip3 install --no-cache-dir pyyaml requests xrootd uproot fast_curator fast_carpenter fast_plotter

#Set Path to x509_cert
ENV WORK_DIR /work
VOLUME ${WORK_DIR}
WORKDIR ${WORK_DIR}
ENV X509_USER_PROXY ${WORK_DIR}/.x509_user_proxy
ENV X509_VOMS_DIR /cvmfs/grid.cern.ch/etc/grid-security/vomsdir
ENV X509_CERT_DIR /cvmfs/grid.cern.ch/etc/grid-security/certificates

#Create non-root user
RUN useradd -ms /bin/bash nonroot
# USER nonroot

CMD ["python3"]
