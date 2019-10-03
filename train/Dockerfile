FROM ubuntu:bionic

RUN apt-get update &&\
    ln -fs /usr/share/zoneinfo/UTC /etc/localtime &&\
    echo "UTC" > /etc/timezone &&\
    DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata &&\
    apt-get install -y r-base-core pandoc && \
    R -q -e 'install.packages(c("rmarkdown", "epiR", "network", "GGally", "e1071"))' &&\
    R -q -e 'install.packages(c("miscTools", "Hmisc", "pROC", "RcppArmadillo"))'

COPY . /app
WORKDIR /app

RUN R -q -e 'install.packages("/app/src/FRESA.CAD_3.0.0.tar.gz", repos = NULL, type="source")'

CMD ['make']


# docker build . -t borre-train
# docker run -v `pwd`/../data_dump:/data_dump -it borre-train /bin/bash