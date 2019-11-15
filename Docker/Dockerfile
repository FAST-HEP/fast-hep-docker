#Base Image
FROM continuumio/miniconda3:4.7.12-alpine
LABEL maintainer="andrew.naylor@sheffield.ac.uk"

USER root
ENV PYTHONDONTWRITEBYTECODE=true

#Install python packages + cleanup
RUN /opt/conda/bin/conda install --yes --freeze-installed \
        -c conda-forge xrootd \
    && /opt/conda/bin/pip install --upgrade pip \
    && /opt/conda/bin/pip install --no-cache-dir \
        pyyaml \
        uproot \
        fast_curator \
        fast_carpenter \
        fast_plotter \
    && /opt/conda/bin/conda clean -afy \
    && find /opt/conda/ -follow -type f -name '*.a' -delete \
    && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
    && find /opt/conda/ -follow -type f -name '*.js.map' -delete \
    && rm -rf /root/.cache/
    
#Set Path to x509_cert
ENV WORK_DIR /work
VOLUME ${WORK_DIR}
WORKDIR ${WORK_DIR}
ENV X509_USER_PROXY ${WORK_DIR}/.x509_user_proxy
ENV X509_VOMS_DIR /cvmfs/grid.cern.ch/etc/grid-security/vomsdir
ENV X509_CERT_DIR /cvmfs/grid.cern.ch/etc/grid-security/certificates

RUN echo ". /opt/conda/etc/profile.d/conda.sh" >> /etc/profile.d/load.sh && \
    echo "conda activate base" >> /etc/profile.d/load.sh

#non-root user
USER 10151
CMD [ "sh", "--login", "-i" ]