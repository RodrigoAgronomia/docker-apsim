FROM ubuntu:18.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=America/Chicago
ENV LC_ALL=C
ENV PATH=/usr/local/APSIMClassic/Model:$PATH
ENV LD_LIBRARY_PATH=/usr/local/APSIMClassic/Model:/lib:/lib64:/usr/lib:/usr/lib64:$LD_LIBRARY_PATH

RUN apt-get update && apt-get install -y \
	apt-transport-https \
	apt-utils \
	software-properties-common \
	add-apt-key
  
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/'
	
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
RUN add-apt-repository 'deb https://download.mono-project.com/repo/ubuntu stable-bionic main'
  
RUN apt-get update && apt-get install -y \
	g++ \
	gfortran \
	mono-devel \
	mono-vbnc

RUN apt-get update && apt-get install -y \
	libboost-all-dev \
	libxml2-dev \
	tcl-dev \
	r-recommended

RUN R --slave -e 'install.packages("rcpp", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("rinside", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("inline", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("xml2", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("snow", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("maps", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("maptools", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("stringr", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("doparallel", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("foreach", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("APSIM", repos="https://cloud.r-project.org/")'
RUN R --slave -e 'install.packages("XML", repos="https://cloud.r-project.org/")'


COPY ./Model usr/local/APSIMClassic/Model
COPY ./Apsim.xml usr/local/APSIMClassic/Apsim.xml
RUN chmod -R a+rwx /usr/local/APSIMClassic
WORKDIR /usr/local/APSIMClassic/Model/Build
RUN sh /usr/local/APSIMClassic/Model/Build/BuildAll.sh || true
RUN ldconfig

ENTRYPOINT [ "mono",  "/usr/local/APSIMClassic/Model/Apsim.exe"]
