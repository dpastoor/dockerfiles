FROM nm74lite:latest 
MAINTAINER "Devin Pastoor" devin.pastoor@gmail.com 

ENV DEBIAN_FRONTEND noninteractive

# setup perl per how docker-perl does it
# NOTE This installs to /usr/local/bin. Debian's perl is at /usr/bin.

RUN apt-get update                                                              \
 && apt-get -y --no-install-recommends install libc6-dev make wget xz-utils \
 && rm -fr /var/lib/apt/lists/*                                                 \
 && wget -q http://www.cpan.org/src/5.0/perl-5.22.0.tar.xz                      \
 && echo be83ead0c5c26cbbe626fa4bac1a4beabe23a9eebc15d35ba49ccde11878e196       \
    perl-5.22.0.tar.xz | sha256sum -c --quiet                                   \
 && tar xJf perl-5.22.0.tar.xz                                                  \
 && cd perl-5.22.0                                                              \
 && ./Configure                                                                 \
    -Accflags="-DNO_MATHOMS -DPERL_DISABLE_PMC"                                 \
    -des                                                                        \
    -Duseshrplib                                                                \
 && make -j`nproc`                                                              \
 && make install                                                                \
 && cd /                                                                        \
 && rm -r perl-5.22.0* usr/local/lib/perl5/5.22.0/pod                           

# cpanm
RUN apt-get update                                                       \
 && apt-get -y --no-install-recommends install ca-certificates           \
 && rm -fr /var/lib/apt/lists/*                                          \
 && wget -qO-                                                            \
    https://raw.githubusercontent.com/miyagawa/cpanminus/master/cpanm |  \
    perl - --skip-satisfied App::cpanminus                               \
 && rm -r ~/.cpanm                                                       
 
 
RUN apt-get update                                                       \
    && apt-get -y --no-install-recommends install build-essential gcc \
    && cpanm \
        Math::Random \
        Statistics::Distributions \
        Archive::Zip \
        File::Copy::Recursive \
        Storable \
        Moose \
        MooseX::Params::Validate  

COPY PsN-Source /tmp/PsN-Source

RUN cd /tmp/PsN-Source  \
    && echo "/opt/PsN/4.6.0/bin\n\
y\n\
/usr/local/bin/perl\n\
/opt/PsN/4.6.0\n\
y\n\
y\n\
n\n\
y\n\
/opt/PsN/4.6.0/test\n\
y\n\
n\n\
nm74\n\
\n\
" | perl setup.pl

ENV PATH /opt/PsN/4.6.0/bin:$PATH

ENTRYPOINT ["/bin/bash"]
