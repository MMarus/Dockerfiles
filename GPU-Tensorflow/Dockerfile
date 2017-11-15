FROM tensorflow/tensorflow:latest-devel-gpu-py3

# pip deps
RUN pip3 --no-cache-dir install \
    xgboost \
    gensim \
    psycopg2 \
    sklearn \
    pymysql \
    keras \
    pandas \
    numpy \
    scipy \
    matplotlib \
    cython \
    h5py

RUN apt-get update && apt-get install -y --no-install-recommends \
    ant \
    wget \
    build-essential \
    curl \
    git \
    ivy \
    python3-dev \
    python3.5 \
    libpython3.5 \
    python3.5-dev \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

#pylucene
RUN echo 'deb http://httpredir.debian.org/debian jessie-backports main' > /etc/apt/sources.list.d/jessie-backports.list
RUN wget -O - https://ftp-master.debian.org/keys/archive-key-8.asc | apt-key add -
RUN wget -O - https://ftp-master.debian.org/keys/archive-key-8-security.asc | apt-key add -
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 8B48AD6246925553
RUN apt-get update \
    && apt-get install -y ca-certificates-java='20161107~bpo8+1' openjdk-8-jdk \
    && apt-get install -y ant

WORKDIR /usr/src/pylucene
RUN curl https://www.apache.org/dist/lucene/pylucene/pylucene-6.5.0-src.tar.gz \
    | tar -xz --strip-components=1

RUN cd /usr/local/lib \
	&& ln -s /usr/lib/python3.5/config-3.5m-x86_64-linux-gnu/libpython3.5.so libpython3.5.so

RUN cd jcc \
    && JCC_JDK=/usr/lib/jvm/java-8-openjdk-amd64 python3 setup.py install
RUN make all install JCC='python3 -m jcc' ANT=ant PYTHON=python NUM_FILES=8

WORKDIR ..
RUN rm -rf pylucene

# the newst sckitlearn
RUN git clone git://github.com/scikit-learn/scikit-learn.git && \
    cd scikit-learn && \
    python3 setup.py build_ext --inplace && \
    pip3 install --editable . && \
    python3 setup.py build_ext --inplace


### GUI OUTPUTS
#RUN apt-get -qq update && apt-get install -y --no-install-recommends  python3-tk
#
### Replace 1000 with your user / group id
#RUN export uid=1000 gid=1000 && \
#    mkdir -p /home/developer && \
#    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
#    echo "developer:x:${uid}:" >> /etc/group && \
#    mkdir /etc/sudoers.d && \
#    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
#    chmod 0440 /etc/sudoers.d/developer && \
#    chown ${uid}:${gid} -R /home/developer
#
#USER developer
#ENV HOME /home/developer


ENTRYPOINT ["bash"]
