FROM ubuntu:bionic

COPY ./src/FRESA.CAD_3.0.0.tar.gz /FRESA.CAD_3.0.0.tar.gz

RUN apt-get update &&\
    ln -fs /usr/share/zoneinfo/UTC /etc/localtime &&\
    echo "UTC" > /etc/timezone &&\
    DEBIAN_FRONTEND=noninteractive apt-get install -y tzdata &&\
    apt-get install -y r-base-core pandoc && \
    R -q -e 'install.packages(c("rmarkdown", "epiR", "network", "GGally", "e1071", "cvTools"))' &&\
    R -q -e 'install.packages(c("miscTools", "Hmisc", "pROC", "RcppArmadillo"))' &&\
    R -q -e 'install.packages("/FRESA.CAD_3.0.0.tar.gz", repos = NULL, type="source")' &&\
    mkdir /app

COPY PredictorsList.txt Makefile /app/
COPY src/ /app/src

WORKDIR /app

CMD ["make", "data_condition"]


# docker build . -t borre-train
# docker run -v `pwd`/../data_dump:/data_dump -it --rm borre-train /bin/bash