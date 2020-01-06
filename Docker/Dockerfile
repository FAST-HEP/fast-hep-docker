#Base Image
FROM continuumio/miniconda3:4.7.12-alpine
LABEL maintainer="andrew.naylor@sheffield.ac.uk"

USER root
ENV PYTHONDONTWRITEBYTECODE=true

#Install python packages + cleanup
RUN apk add --no-cache bash && \
    opt/conda/bin/conda install --yes --freeze-installed \
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

#Create conda loading script and change permissions 
RUN echo " " > /home/anaconda/.profile
RUN echo ". /opt/conda/etc/profile.d/conda.sh" >> /etc/profile.d/load.sh && \
    echo "conda activate base" >> /etc/profile.d/load.sh
RUN chmod +x /etc/profile.d/load.sh

#non-root user
USER 10151
# ENV PATH "/bin:/sbin:/usr/bin"
# ENV PATH "/opt/conda/bin:/opt/conda/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"


CMD [ "/bin/bash", "--login", "-i" ]

# PATH = /opt/conda/bin:/opt/conda/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

###### cat /home/anaconda/.profile ####
#. /opt/conda/etc/profile.d/conda.sh
# conda activate base

###### cat /opt/conda/etc/profile.d/conda.sh ####
# export CONDA_EXE='/opt/conda/bin/conda'
# export _CE_M=''
# export _CE_CONDA=''
# export CONDA_PYTHON_EXE='/opt/conda/bin/python'

# # Copyright (C) 2012 Anaconda, Inc
# # SPDX-License-Identifier: BSD-3-Clause

# __add_sys_prefix_to_path() {
#     # In dev-mode CONDA_EXE is python.exe and on Windows
#     # it is in a different relative location to condabin.
#     if [ -n "${_CE_CONDA}" ] && [ -n "${WINDIR+x}" ]; then
#         SYSP=$(\dirname "${CONDA_EXE}")
#     else
#         SYSP=$(\dirname "${CONDA_EXE}")
#         SYSP=$(\dirname "${SYSP}")
#     fi

#     if [ -n "${WINDIR+x}" ]; then
#         PATH="${SYSP}/bin:${PATH}"
#         PATH="${SYSP}/Scripts:${PATH}"
#         PATH="${SYSP}/Library/bin:${PATH}"
#         PATH="${SYSP}/Library/usr/bin:${PATH}"
#         PATH="${SYSP}/Library/mingw-w64/bin:${PATH}"
#         PATH="${SYSP}:${PATH}"
#     else
#         PATH="${SYSP}/bin:${PATH}"
#     fi
#     \export PATH
# }

# __conda_hashr() {
#     if [ -n "${ZSH_VERSION:+x}" ]; then
#         \rehash
#     elif [ -n "${POSH_VERSION:+x}" ]; then
#         :  # pass
#     else
#         \hash -r
#     fi
# }

# __conda_activate() {
#     if [ -n "${CONDA_PS1_BACKUP:+x}" ]; then
#         # Handle transition from shell activated with conda <= 4.3 to a subsequent activation
#         # after conda updated to >= 4.4. See issue #6173.
#         PS1="$CONDA_PS1_BACKUP"
#         \unset CONDA_PS1_BACKUP
#     fi

#     \local cmd="$1"
#     shift
#     \local ask_conda
#     OLDPATH="${PATH}"
#     __add_sys_prefix_to_path
#     ask_conda="$(PS1="$PS1" "$CONDA_EXE" $_CE_M $_CE_CONDA shell.posix "$cmd" "$@")" || \return $?
#     PATH="${OLDPATH}"
#     \eval "$ask_conda"
#     __conda_hashr
# }

# __conda_reactivate() {
#     \local ask_conda
#     OLDPATH="${PATH}"
#     __add_sys_prefix_to_path
#     ask_conda="$(PS1="$PS1" "$CONDA_EXE" $_CE_M $_CE_CONDA shell.posix reactivate)" || \return $?
#     PATH="${OLDPATH}"
#     \eval "$ask_conda"
#     __conda_hashr
# }

# conda() {
#     if [ "$#" -lt 1 ]; then
#         "$CONDA_EXE" $_CE_M $_CE_CONDA
#     else
#         \local cmd="$1"
#         shift
#         case "$cmd" in
#             activate|deactivate)
#                 __conda_activate "$cmd" "$@"
#                 ;;
#             install|update|upgrade|remove|uninstall)
#                 OLDPATH="${PATH}"
#                 __add_sys_prefix_to_path
#                 "$CONDA_EXE" $_CE_M $_CE_CONDA "$cmd" "$@"
#                 \local t1=$?
#                 PATH="${OLDPATH}"
#                 if [ $t1 = 0 ]; then
#                     __conda_reactivate
#                 else
#                     return $t1
#                 fi
#                 ;;
#             *)
#                 OLDPATH="${PATH}"
#                 __add_sys_prefix_to_path
#                 "$CONDA_EXE" $_CE_M $_CE_CONDA "$cmd" "$@"
#                 \local t1=$?
#                 PATH="${OLDPATH}"
#                 return $t1
#                 ;;
#         esac
#     fi
# }

# if [ -z "${CONDA_SHLVL+x}" ]; then
#     \export CONDA_SHLVL=0
#     # In dev-mode CONDA_EXE is python.exe and on Windows
#     # it is in a different relative location to condabin.
#     if [ -n "${_CE_CONDA+x}" ] && [ -n "${WINDIR+x}" ]; then
#         PATH="$(\dirname "$CONDA_EXE")/condabin${PATH:+":${PATH}"}"
#     else
#         PATH="$(\dirname "$(\dirname "$CONDA_EXE")")/condabin${PATH:+":${PATH}"}"
#     fi
#     \export PATH

#     # We're not allowing PS1 to be unbound. It must at least be set.
#     # However, we're not exporting it, which can cause problems when starting a second shell
#     # via a first shell (i.e. starting zsh from bash).
#     if [ -z "${PS1+x}" ]; then
#         PS1=
#     fi
# fi

