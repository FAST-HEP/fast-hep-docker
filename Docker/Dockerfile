#Base Image
FROM python:3.7.4-alpine3.10
LABEL maintainer="andrew.naylor@sheffield.ac.uk"

USER root

#re-order the lines of code into the fewest stages but once it's all been built properly

#Install bash + numpy + pandas 
RUN apk add --no-cache bash python3-dev libstdc++ && \
    apk add --no-cache g++ && \
    ln -s /usr/include/locale.h /usr/include/xlocale.h && \
    pip3 install numpy && \
    pip3 install pandas
#In one step as it takes a long time to build numpy + pandas
# apk add --no-cache libpng libpng-dev freetype freetype-dev && \

#Install XRootD + pxyrootd 
RUN apk add --no-cache libpng libpng-dev freetype freetype-dev llvm llvm-dev 
RUN apk add --no-cache make cmake git curl openssl
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
# RUN pip3 install --no-cache-dir six pyyaml XRootD uproot fast_curator fast_carpenter fast_plotter
RUN pip3 install --no-cache-dir pyyaml uproot fast_curator fast_carpenter fast_plotter

#Set Path to x509_cert
ENV WORK_DIR /work
VOLUME ${WORK_DIR}
WORKDIR ${WORK_DIR}
ENV X509_USER_PROXY ${WORK_DIR}/.x509_user_proxy
ENV X509_VOMS_DIR /cvmfs/grid.cern.ch/etc/grid-security/vomsdir
ENV X509_CERT_DIR /cvmfs/grid.cern.ch/etc/grid-security/certificates

#Add Docker entrypoint
# COPY docker-entrypoint.sh /docker-entrypoint.sh
# RUN chmod +x /docker-entrypoint.sh

#Create non-root user
ARG USERID=1000
RUN adduser -D -u ${USERID} nonroot
# USER nonroot

# ENTRYPOINT ["./docker-entrypoint.sh"]
CMD ["python3"]
