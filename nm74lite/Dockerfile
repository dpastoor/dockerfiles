FROM debian:testing 
MAINTAINER Devin Pastoor 

# Update/upgrade
RUN apt-get update
RUN apt-get upgrade -y


# Install gfortran, wget, and unzip (then clean up the image
# as much as possible)
RUN apt-get update \
    && apt-get install --yes --no-install-recommends \
       gcc \
       gfortran \
       libmpich-dev \
       mpich \
       wget \
       unzip \
       vim-tiny \
    && rm -rf /var/lib/apt/lists/ \
              /var/cache/apt/archives/ \
	      /usr/share/doc/ \
	      /usr/share/man/ \
	      /usr/share/locale/

# Add NONMEM Files and install then add nonmem to path
COPY nm74CD06AUG2016 /tmp/nm740CD
RUN mkdir -p /opt/NONMEM/nm74
COPY nonmem.lic /opt/NONMEM/nm74/license/nonmem.lic
RUN bash /tmp/nm740CD/SETUP74 /tmp/nm740CD /opt/NONMEM/nm74 gfortran y /usr/bin/ar same rec i unzip nonmem74e.zip nonmem74r.zip \ 
    && rm -rf /tmp/* \
    && rm /opt/NONMEM/nm74/mpi/mpi_ling/libmpich.a \
    && ln -s /usr/lib/mpich/lib/libmpich.a /opt/NONMEM/nm74/mpi/mpi_ling/libmpich.a \
    && (cd /opt/NONMEM/nm74 && \
        rm -rf \
            examples/ \
            guides/ \
            help/ \
            html/ \
            *.pdf \
            *.txt \
            *.zip \
            install* \
            nonmem.lic \
            SETUP* \
            unzip.SunOS \
            unzip.exe \
            mpi/mpi_lini \
            mpi/mpi_wing \
            mpi/mpi_wini \
            run/*.bat \
            run/*.EXE \
            run/*.LNK \
            run/CONTROL* \
            run/DATA* \
            run/REPORT* \
            run/fpiwin* \
            run/mpiwin* \
            run/FCON \
            run/FDATA \
            run/FREPORT \
            run/FSIZES \
            run/FSTREAM \
            run/FSUBS \
            run/INTER \
            run/computername.exe \
            run/garbage.out \
            run/gfortran.txt \
            run/nmhelp.exe \
            run/psexec.exe \
            runfiles/GAWK.EXE \
            runfiles/GREP.EXE \
            runfiles/computername.exe \
            runfiles/fpiwin* \
            runfiles/mpiwin* \
            runfiles/nmhelp.exe \
            runfiles/psexec.exe \
            util/*.bat \
            util/*~ \
            util/CONTROL* \
            util/F* \
            util/DATA3 \
            util/ERROR1 \
            util/INTER \
            util/finish_Darwin* \
            util/finish_Linux_f95 \
            util/finish_Linux_g95 \
            util/finish_SunOS*)

env PATH $PATH:/opt/NONMEM/nm74/run       
  
ENTRYPOINT ["/bin/bash"]
