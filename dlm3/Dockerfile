# docker build -t activeeon/dlm3:latest .
# docker run -it --rm activeeon/dlm3 bash
FROM python:3.6

WORKDIR /tmp

RUN apt-get clean && apt-get update -y -qq

# Install generic packages
# ------------------------
RUN apt-get install -y \
        build-essential \
        cmake \
        pkg-config \
        wget \
        swig \
        git \
        curl \
        unzip \
        libaio1 \
        nano \
        freetds-dev \
        unixodbc \
        unixodbc-dev \
        libjpeg-dev \
        libtiff5-dev \
        libpng-dev \
        libgtk2.0-dev \
        libavcodec-dev \
        libavformat-dev \
        libswscale-dev \
        libv4l-dev \
        libatlas-base-dev \
        gfortran \
        libhdf5-dev \
        libtbb2 \
        libtbb-dev \
        libgl1-mesa-glx

RUN apt-get install openmpi-bin -y

# Install java
# ------------
RUN apt-get install -y openjdk-11-jdk
RUN apt-get install ca-certificates-java && update-ca-certificates -f
ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk-amd64/
RUN export JAVA_HOME

# Install python libraries
# ------------------------
RUN pip install py4j
RUN pip install cython

RUN pip install tensorflow keras
RUN pip install scikit-learn
RUN pip install pandas
#RUN pip install pandas-ml pandasql
#RUN pip install pymc3
RUN pip install networkx nltk statsmodels luminol tsfresh

#RUN pip install http://download.pytorch.org/whl/cpu/torch-0.4.1-cp36-cp36m-linux_x86_64.whl
RUN pip install https://download.pytorch.org/whl/cpu/torch-1.0.1.post2-cp36-cp36m-linux_x86_64.whl
#RUN pip install https://download.pytorch.org/whl/cpu/torch-1.1.0-cp36-cp36m-linux_x86_64.whl
RUN pip install torchvision==0.2.2.post2
RUN pip install torchtext gensim implicit

RUN pip install scikit-image graphviz opencv-contrib-python-headless
RUN pip install bokeh
RUN pip install boto boto3 bz2file s3fs
RUN pip install Pillow==6.1

RUN pip install tensorboardX

RUN pip install cassandra-driver cx-Oracle elasticsearch pymongo pymssql PyMySQL pyodbc pyOpenSSL SecretStorage
RUN pip install mysql-connector-python==8.0.17
RUN pip install sqlalchemy==1.3.5
RUN pip install snowflake-sqlalchemy
RUN pip install snowflake-connector-python[pandas]

RUN pip install tabulate colorama future
RUN pip install -f http://h2o-release.s3.amazonaws.com/h2o/latest_stable_Py.html h2o

RUN pip install beautifulsoup4 tqdm dill
RUN pip install psycopg2 psycopg2-binary

RUN pip install pantab==0.0.1b5
RUN pip install tableauserverclient

# azure-cognitiveservices-vision-face
RUN pip install azure-cognitiveservices-vision-computervision azure-cognitiveservices-vision-contentmoderator azure-cognitiveservices-vision-customvision azure-cognitiveservices-search-websearch azure-cognitiveservices-search-imagesearch azure-cognitiveservices-search-entitysearch azure-cognitiveservices-search-customsearch azure-cognitiveservices-search-visualsearch azure-cognitiveservices-search-videosearch azure-cognitiveservices-search-newssearch  azure-cognitiveservices-language-textanalytics azure-cognitiveservices-language-spellcheck azure-mgmt-cognitiveservices
RUN pip install azure-mgmt-machinelearningcompute azure-mgmt-batchai
RUN pip install azureml-sdk==1.0.48

#RUN pip install cntk
#RUN pip install mxnet
#RUN pip install pyspark

RUN pip install xgboost catboost
RUN pip install scikit-learn==0.21  # to be compatible with auto-sklearn
RUN pip install tpot==0.11.0  # to be compatible with scikit-learn==0.21
RUN pip install auto-sklearn
# https://stackoverflow.com/questions/43124775/why-python-3-6-1-throws-attributeerror-module-enum-has-no-attribute-intflag
# Since python 3.6 the enum34 library is no longer compatible with the standard library.
#RUN pip uninstall -y enum34  # pandas-ml
RUN pip install git+https://github.com/ow2-proactive/chocolate@master
RUN pip install ghalton
RUN pip install hyperopt

RUN pip install dask distributed
RUN pip install dlib

RUN pip install wget lxml validators

RUN pip install eli5
#RUN pip uninstall matplotlib
RUN pip install matplotlib==2.2.2
RUN pip install mpld3
RUN pip install shap==0.30.0
RUN pip install pdpbox

#RUN pip install flask flask-cors
#RUN pip install Werkzeug==0.16.1
#RUN pip install connexion[swagger-ui]==2.6.0

#RUN pip uninstall numpy -y
#RUN rm -rf /usr/local/lib/python3.6/site-packages/numpy
#RUN pip install numpy==1.16.4

RUN pip install proactive

#RUN pip install jupyter jupyterlab
#RUN pip install proactive-jupyter-kernel --upgrade
#RUN python -m proactive-jupyter-kernel.install

# Install Visdom (Facebook Research) from source
# ----------------------------------------------
WORKDIR /tmp
RUN git clone https://github.com/facebookresearch/visdom.git
RUN cd visdom && pip install -e . && easy_install .
#RUN rm -rf visdom

# Install ODPI-C libraries for Oracle xe-11g database
# ----------------------------------------------
WORKDIR /
COPY instantclient-basic-linux.x64-11.2.0.4.0.zip /
RUN mkdir -p /opt/oracle && unzip instantclient-basic-linux.x64-11.2.0.4.0.zip  &&  mv instantclient_11_2/ /opt/oracle/ && rm  instantclient-basic-linux.x64-11.2.0.4.0.zip 
RUN sh -c "echo /opt/oracle/instantclient_11_2 > /etc/ld.so.conf.d/oracle-instantclient.conf"
RUN ldconfig

# Install Tableau Extract API
#------------------------------------------------
RUN wget -O SDK.tar.gz "https://downloads.tableau.com/tssoftware/extractapi-py-linux-x86_64-2019-1-2.tar.gz" \
      && mkdir -p /sdk \
      && tar -xzf SDK.tar.gz -C /sdk --strip-components=1 \
      && rm SDK.tar.gz \
      && cd /sdk \
      && python setup.py install \
      && rm -rf /sdk

RUN rm -rf ~/.cache/pip

RUN apt-get autoclean && apt-get clean \
        && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
