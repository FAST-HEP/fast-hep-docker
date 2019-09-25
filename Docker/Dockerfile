#Base Image
FROM centos:7.4.1708
LABEL maintainer="andrew.naylor@sheffield.ac.uk"

USER root

#Install Python + pxyrootd prerequisites
RUN yum install -y centos-release-scl \
    && yum-config-manager --enable rhel-7-server-optional-rpms \
    && yum install -y epel-release \
    && yum install -y cmake make zlib-devel gcc-c++ openssl-devel \
    && yum install -y python-devel rh-python35 rh-python35-python-devel rh-python35-python-setuptools xrootd-client-devel \
    && yum clean all \
    && rm -rf /var/cache/yum

#Install pip packages
RUN source /opt/rh/rh-python35/enable \
    && curl https://bootstrap.pypa.io/get-pip.py | python3 \
    && pip3 install --no-cache-dir --upgrade setuptools \    
    && pip3 install --no-cache-dir six pyyaml XRootD uproot fast_curator fast_carpenter fast_plotter

# make some useful symlinks that are expected to exist
RUN source /opt/rh/rh-python35/enable \
  && cd /usr/local/bin \
	&& ln -s idle3 idle \
	&& ln -s pydoc3 pydoc \
	&& ln -s python3 python \
	&& ln -s python3-config python-config

#Set Path to x509_cert
ENV WORK_DIR /work
VOLUME ${WORK_DIR}
WORKDIR ${WORK_DIR}
ENV X509_USER_PROXY ${WORK_DIR}/.x509_user_proxy
ENV X509_VOMS_DIR /cvmfs/grid.cern.ch/etc/grid-security/vomsdir
ENV X509_CERT_DIR /cvmfs/grid.cern.ch/etc/grid-security/certificates

# Add Python3 source script
COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh

#Create non-root user
ARG USERID=1000
RUN adduser --uid ${USERID} nonroot
USER nonroot

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["python3"]
