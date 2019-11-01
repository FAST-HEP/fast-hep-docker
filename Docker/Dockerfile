#Base Image
FROM continuumio/miniconda3:4.7.12-alpine
# FROM continuumio/miniconda3:4.7.12
#file size in debian is 955MB
LABEL maintainer="andrew.naylor@sheffield.ac.uk"

#Install python packages
RUN . ~/.profile && \ 
    conda install -c conda-forge xrootd && \
    pip install --no-cache-dir six pyyaml requests uproot fast_curator fast_carpenter fast_plotter && \
    conda clean -afy

#Set Path to x509_cert
ENV WORK_DIR /work
VOLUME ${WORK_DIR}
WORKDIR ${WORK_DIR}
ENV X509_USER_PROXY ${WORK_DIR}/.x509_user_proxy
ENV X509_VOMS_DIR /cvmfs/grid.cern.ch/etc/grid-security/vomsdir
ENV X509_CERT_DIR /cvmfs/grid.cern.ch/etc/grid-security/certificates

# CMD ["python3"]
CMD [ "sh", "--login", "-i" ]
