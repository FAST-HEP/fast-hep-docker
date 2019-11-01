#Base Image
FROM continuumio/miniconda3:4.7.12-alpine
LABEL maintainer="andrew.naylor@sheffield.ac.uk"

USER root

#Copy Across conda env file + entrypoint
COPY environment.yml /environment.yml
COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh

#Install python packages + cleanup
RUN . /opt/conda/etc/profile.d/conda.sh && \ 
    conda env create -f /environment.yml && \
    conda env list && \
    conda clean -afy && \
    rm -rf /root/.cache/

#Set Path to x509_cert
ENV WORK_DIR /work
VOLUME ${WORK_DIR}
WORKDIR ${WORK_DIR}
ENV X509_USER_PROXY ${WORK_DIR}/.x509_user_proxy
ENV X509_VOMS_DIR /cvmfs/grid.cern.ch/etc/grid-security/vomsdir
ENV X509_CERT_DIR /cvmfs/grid.cern.ch/etc/grid-security/certificates

#non-root user
USER anaconda

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["python"]
